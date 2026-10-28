# Master Technical Interview Preparation Guide

**Author:** Nithishvelan M ([@ErenYeager1836](https://github.com/ErenYeager1836))  
**Focus Areas:** Systems C++, Concurrency, Game Engine Architecture, Application Security (AppSec)  

---

## 1. Systems & C++ Interview Questions

### Q1: Why did you use Lock Striping instead of a single `std::shared_mutex` in your KV Store?
* **Answer:** A single `std::shared_mutex` serializes all write operations globally. Even if two threads write to completely unrelated keys, they must contend for the same cache line containing the mutex atomic state. This causes cache ping-pong, CPU pipeline stalls, and severely degrades $p99$ tail latency.
* **The Solution:** By partitioning the key space into 64 distinct shards and hashing keys with `std::hash<std::string>{}(key) & 63`, concurrent writes accessing different keys have a ~98.4% probability of running in parallel with zero contention. In benchmarks, this scaled throughput from 92,000 ops/sec (single thread) to **425,000 ops/sec** across 16 threads.

### Q2: What is the difference between AddressSanitizer (ASan) and Valgrind? When do you use each?
* **Answer:**
  * **AddressSanitizer (ASan):** Uses compile-time instrumentation and shadow memory to track pointer validity. It detects out-of-bounds accesses (heap, stack, global), use-after-free, and double frees with minimal slowdown (~2x). It is ideal for continuous integration (CI) test matrices.
  * **Valgrind Memcheck:** Uses dynamic binary translation (interpreting x86-64 machine instructions at runtime) without recompilation. While it has a much higher performance overhead (~10x to 30x slowdown), it accurately identifies uninitialized memory reads and provides detailed heap block allocation graphs (via Massif).
* **In Practice:** I run ASan and UBSan directly in GitHub Actions on every pull request, and use Valgrind locally for periodic definitive leak verification.

### Q3: Explain RAII and how move semantics prevent unnecessary heap allocations.
* **Answer:** Resource Acquisition Is Initialization (RAII) ties resource lifespan (file descriptors, sockets, mutex locks, heap memory) to object lifetime. When an object leaves scope, its destructor deterministically reclaims the resource.
* **Move Semantics (C++11):** Instead of deep-copying heap buffers (`std::vector` or `std::string`), rvalue references (`T&&`) and `std::move` transfer ownership of the underlying raw pointer, setting the source pointer to `nullptr`. This reduces copy overhead from $O(N)$ allocation to an $O(1)$ pointer transfer.

---

## 2. Game Development & Engine Interview Questions

### Q1: Why is an Entity Component System (ECS) using Sparse Sets superior to traditional OOP inheritance?
* **Answer:** Traditional OOP creates deep inheritance trees (`GameObject -> Actor -> Monster`) stored via pointer collections (`std::vector<GameObject*>`). This causes:
  1. **Pointer chasing:** Objects are scattered randomly across heap pages, inducing L1/L2 cache misses.
  2. **Cache line waste:** Fetching a 64-byte cache line to access an 8-byte `Position` brings in unused variables (sound pointers, mesh data).
  3. **Virtual call overhead:** Dynamic dispatch stalls CPU branch predictors.
* **The Sparse Set Fix:** In our ECS (`game-dev-labs`), components are stored in packed, contiguous memory (`std::vector<Component>`). The system iterates sequentially over dense arrays with near 100% hardware prefetcher hit rates. Deletion uses the **swap-and-pop** idiom in $O(1)$ time without introducing fragmentation.

### Q2: How does Glenn Fiedler's "Fix Your Timestep!" accumulator work?
* **Answer:** Physics simulations become unstable when integrated with variable frame times ($\Delta t$). If the frame rate drops, large $\Delta t$ steps cause tunneling (objects passing through walls).
* **The Accumulator:**
  1. Measure elapsed real time since the last frame and add it to an `accumulator`.
  2. While `accumulator >= FIXED_DT` (e.g., $1/60$s), run the physics simulation with a constant time step (`Integrate(FIXED_DT)`), subtracting `FIXED_DT` each step.
  3. Calculate a remainder fraction $\alpha = \frac{\text{accumulator}}{\text{FIXED\_DT}}$.
  4. Render the graphics by linearly interpolating between the previous physics state and the current physics state using $\alpha$. This guarantees perfectly smooth rendering at 144Hz while physics executes deterministically at 60Hz.

### Q3: Explain how the Separating Axis Theorem (SAT) detects convex polygon collisions.
* **Answer:** SAT states that two convex shapes do not intersect if there exists an axis onto which the projections of the two shapes do not overlap.
* **Algorithm:**
  1. Compute the surface normal vectors for all edges of both polygons (the candidate separating axes).
  2. Project all vertices of Polygon A and Polygon B onto each axis by taking the dot product.
  3. Check for overlap between the minimum and maximum scalar projection values.
  4. If an axis has **zero overlap**, the shapes are separated (early return false).
  5. If all axes overlap, the shapes collide. The axis with the **minimum overlap** represents the Minimum Translation Vector (MTV), used to push the objects apart and calculate impulse forces.

---

## 3. Application Security (AppSec) Interview Questions

### Q1: Why is an AST-based SAST scanner superior to Regular Expressions (Regex)?
* **Answer:** Regex pattern matching has syntactic blindness:
  1. **Comments & Docstrings:** A comment saying `# Do not use pickle.loads()` triggers a false alert in regex linters. AST parsers ignore comments and docstrings during tokenization.
  2. **Keyword vs Positional Arguments:** In Python, functions can be invoked as `subprocess.Popen(cmd, shell=True)` or `subprocess.Popen(cmd, bufsize=0, executable=None, shell=False)`. Regex cannot reliably evaluate arbitrary argument permutations across multi-line breaks.
  3. **Syntactic Context:** AST analysis evaluates the node type (`ast.Call`), function attributes (`node.func.attr == 'Popen'`), and inspects parameter constants (`keyword.arg == 'shell' and keyword.value.value is True`).

### Q2: What is Shannon Entropy and how does your scanner use it to detect secrets?
* **Answer:** Claude Shannon's Information Entropy formula measures the average information content or unpredictability of a string:
  $$H(X) = -\sum_{i=1}^{n} P(x_i) \log_2 P(x_i)$$
* **Application in `secret-scanner`:** Human-written code and identifiers have low entropy ($H < 2.8$) due to repetitive character frequencies. High-entropy cryptographic secrets (AWS secret keys, base64 tokens, 256-bit private keys) exhibit uniform character distributions resulting in high entropy ($H \ge 3.2$). By sliding a window across strings and checking both pattern regex and entropy thresholds, we detect unpatterned API tokens with minimal false positives.

### Q3: What is OASIS SARIF and why is it important in modern DevSecOps?
* **Answer:** Static Analysis Results Interchange Format (SARIF 2.1.0) is a standardized JSON schema for static analysis tools. 
* **Importance:** Historically, every security tool produced proprietary JSON/XML outputs requiring bespoke parsers. SARIF provides a universal schema that GitHub Code Scanning, GitLab CI, and IDEs natively render into pull request annotations and security dashboard alerts. By designing `ast-guard` to emit valid SARIF 2.1.0, it integrates into enterprise CI/CD gates out of the box.
