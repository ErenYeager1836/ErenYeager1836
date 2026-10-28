# Nithishvelan M (ErenYeager1836)

**Systems & Application Security Engineer**  
Specializing in C++17/20, Linux Kernel Subsystems, Binary ABI Emulation, and Static Application Security Testing (SAST).

[![GitHub](https://img.shields.io/badge/GitHub-ErenYeager1836-181717?style=flat&logo=github)](https://github.com/ErenYeager1836)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Nithishvelan--M-0A66C2?style=flat&logo=linkedin)](https://www.linkedin.com/in/nithishvelan-m-096847263/)
[![Email](https://img.shields.io/badge/Email-mnithishvelan%40gmail.com-D14836?style=flat&logo=gmail)](mailto:mnithishvelan@gmail.com)

---

## 🚀 High-Impact Upstream Open Source Contributions

| Project | Subsystem / Focus | Technical Contribution & Impact | Case Study |
| :--- | :--- | :--- | :---: |
| **[ValveSoftware/gamescope](https://github.com/ValveSoftware/gamescope)** | Vulkan 1.3 / PipeWire | Resolved NV12 multi-planar storage image validation and DRM modifier alignment on NVIDIA drivers, eliminating video corruption during Steam Remote Play. | [Read RCA](docs/engineering-notes/01-gamescope-vulkan-nv12-remote-play.md) |
| **[ValveSoftware/Proton](https://github.com/ValveSoftware/Proton)** | Wine / POSIX Filesystem | Corrected DriverStore directory case-handling and file descriptor lifecycle to eliminate Linux case-sensitivity collisions during Windows DLL resolution. | — |
| **[boykopovar/AnyPS5](https://github.com/boykopovar/AnyPS5)** | PRX Runtime & ABI | Implemented `sceSysmoduleIsLoadedInternal` lifecycle tracking (*merged*), added FreeBSD `tgammaf` floating-point math, and audited PRX stubs against strict Python AST header CI. | [Read RCA](docs/engineering-notes/02-anyps5-abi-prx-runtime-verification.md) |
| **[Linux Kernel](https://git.kernel.org)** | Staging / IIO (`ad5933`) | Refactored multi-branch driver locking in `ad5933_show()` to scoped `guard(mutex)` semantics, structurally eliminating error-path lock leakage. | [Read RCA](docs/engineering-notes/03-linux-kernel-scoped-guard-refactoring.md) |
| **[GitHub Advisory DB](https://github.com/github/advisory-database)** | Security Research | Security advisory documentation enhancements and vulnerability scope clarification for published CVEs. | — |

---

## 🔬 Systems Engineering Post-Mortems & Notes

Deep dives into complex production debugging, concurrency design, and low-level platform architecture:

* 📄 **[Vulkan NV12 Storage Image Corruption in Gamescope](docs/engineering-notes/01-gamescope-vulkan-nv12-remote-play.md)**: Format capability negotiation across Mesa vs. NVIDIA proprietary Vulkan drivers for Steam Remote Play streaming.
* 📄 **[PS5 PRX ABI Emulation & Enforcing Static Header Invariants](docs/engineering-notes/02-anyps5-abi-prx-runtime-verification.md)**: "Fail-loud" throwing stubs vs. silent no-ops and enforcing single-declaration rules across 12,000+ exports via Python AST static analysis.
* 📄 **[Modernizing Linux Kernel Locking with RAII `guard(mutex)`](docs/engineering-notes/03-linux-kernel-scoped-guard-refactoring.md)**: Eliminating kernel deadlock vulnerabilities using GCC/Clang `__cleanup__` scope-based RAII in Linux Staging drivers.

---

## 🛠️ Featured Systems & Security Architecture

### 🔍 [ast-guard](https://github.com/ErenYeager1836/ast-guard) — AST-Based SAST Engine & GitHub Action
* **Architecture:** Custom Python Abstract Syntax Tree (AST) visitor that performs syntactic security audits without the false-positive noise of regex pattern matching.
* **Telemetry:** Emits structured **SARIF 2.1.0** reports for direct ingestion into GitHub Code Scanning security alerts.
* **Performance:** Benchmarked against repositories with 100k+ LOC; completes full rule evaluation in `< 1.2s`.
* **CI/CD Integration:** Packaged as an enterprise-ready **GitHub Action** (`ErenYeager1836/ast-guard@v1`).
* **Design Documentation:** [ADR 0001: AST Visitor Traversal vs. Regular Expression Matching](https://github.com/ErenYeager1836/ast-guard/blob/main/docs/adr/0001-ast-visitor-vs-regex-sast.md).

### ⚡ [mini-projects (Systems Lab)](https://github.com/ErenYeager1836/mini-projects) — High-Concurrency C++17 Core
* **In-Memory Key-Value Store:** Multi-threaded storage engine featuring lock-striped hash partitioning, Append-Only File (AOF) persistence, and custom RESP-like binary protocol.
* **Sliding-Window Rate Limiter:** High-throughput token-bucket and sliding-window rate limiters with millisecond timestamp precision.
* **Benchmarks:** Microbenchmarked via Google Benchmark: sustains **420,000+ ops/sec** at `< 0.18ms p99 latency` under multi-threaded contention.
* **Memory Safety:** Audited with AddressSanitizer (ASan), UndefinedBehaviorSanitizer (UBSan), and ThreadSanitizer (TSan); zero memory leaks verified via Valgrind.
* **Design Documentation:** [ADR 0001: Lock Striping vs. Global Mutex Partitioning](https://github.com/ErenYeager1836/mini-projects/blob/main/kv-store/docs/adr/0001-lock-striping-vs-global-mutex.md).

### 🔑 [secret-scanner](https://github.com/ErenYeager1836/secret-scanner) — High-Entropy Credential Detection
* **Mathematical Model:** Employs Shannon entropy calculation over sliding character windows to distinguish high-randomness cryptographic secrets from normal code.
* **Audit Coverage:** Audits working directories, staged index buffers, and entire Git commit history DAGs without leaking decrypted tokens to stdout.
* **Benchmarking:** Throughput exceeds **85 MB/s** scanning large codebases with `< 45MB` memory footprint.

### 🎮 [game-dev-labs](https://github.com/ErenYeager1836/game-dev-labs) — Data-Oriented C++17 Game Architecture
* **Entity Component System (ECS):** Implemented using the **Sparse Set** pattern for cache-line dense iteration and $O(1)$ component addition/removal with zero vtable pointer chasing.
* **Physics & Math:** 2D Separating Axis Theorem (SAT) convex collision detection, A* procedural pathfinding, and deterministic fixed-timestep state machine.
* **Design Documentation:** [ADR 0001: Sparse Sets vs. Contiguous Dense Arrays in ECS](https://github.com/ErenYeager1836/game-dev-labs/blob/main/docs/adr/0001-sparse-set-vs-dense-array-ecs.md).

### 📡 [reconflow](https://github.com/ErenYeager1836/reconflow) — Reconnaissance Orchestration CLI
* **Corpus Recognition:** Selected and indexed by the **XERJ Corpus Hub** as a reference implementation for coding agents.
* **Capabilities:** Concurrent subdomain enumeration, DNS resolution, and HTTP probing published as an installable package on PyPI.

---

## 📐 Engineering Discipline & Quality Standards

* **Sanitizer-Driven Quality:** All C++ codebases are tested in automated CI matrices under **AddressSanitizer (ASan)**, **UndefinedBehaviorSanitizer (UBSan)**, and **ThreadSanitizer (TSan)**.
* **Architectural Decision Records (ADRs):** Major design trade-offs (memory layout, algorithmic complexity, threading models) are formally documented using ADR structures.
* **Standardized Telemetry:** Security tooling natively conforms to OASIS **SARIF 2.1.0** specifications for CI/CD DevSecOps interoperability.

---

## 💻 Technical Stack & Competencies

* **Languages:** C++17/20, C, Python 3, POSIX Shell (Bash), SQL
* **Systems & Graphics:** Linux Kernel Internals, Vulkan 1.3, POSIX Sockets, Multi-threading (pthreads/std::thread), Memory Management, CMake, Valgrind, GDB
* **Security & DevSecOps:** Static Analysis (SAST), SARIF 2.1.0, Threat Modeling, Shannon Entropy Heuristics, OWASP Top 10, Docker, CI/CD GitHub Actions
* **Tools & Toolchains:** Clang/LLVM, GCC, MSVC, Git Internals, Google Benchmark, Pytest, Flake8, MkDocs
