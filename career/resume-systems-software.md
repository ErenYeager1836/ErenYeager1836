# Nithishvelan M
**Systems Software Engineer**  
Chennai, India | [mnithishvelan@gmail.com](mailto:mnithishvelan@gmail.com) | [github.com/ErenYeager1836](https://github.com/ErenYeager1836) | [linkedin.com/in/nithishvelan-m-096847263](https://www.linkedin.com/in/nithishvelan-m-096847263/)

---

### PROFESSIONAL SUMMARY
Systems Software Engineer specializing in modern C++17/20, Linux kernel subsystems, multi-threaded concurrency, low-latency in-memory data structures, and cross-platform POSIX networking. Upstream contributor to the Linux Kernel, Valve’s Linux infrastructure (`gamescope`, `Proton`), and binary compatibility runtimes (`AnyPS5`). Demonstrated expertise in lock-striping optimization, memory profiling, and building production-hardened systems with automated multi-OS CI/CD matrices.

---

### CORE TECHNICAL SKILLS
* **Languages & Compilers:** C++17, C++20, C, Python 3, POSIX Bash, Clang/LLVM, GCC, MSVC, CMake.
* **Concurrency & Systems:** Multi-threading (pthreads/`std::thread`), Reader-Writer Locks (`std::shared_mutex`), Lock Striping, Atomics, RAII Resource Management, Memory Alignment & Cache Coherency, POSIX Sockets, Winsock2.
* **Linux Kernel & Low-Level:** Linux Driver Architecture, `<linux/cleanup.h>`, Scoped Guards, Virtual ABI (`APS5_VABI`), ELF Binary Structure, Memory Profiling (Valgrind Massif/Memcheck), Google Benchmark.
* **Quality & CI/CD:** AddressSanitizer (ASan), UndefinedBehaviorSanitizer (UBSan), ThreadSanitizer (TSan), GitHub Actions Matrix (Ubuntu, Windows, macOS), Docker.

---

### UPSTREAM OPEN SOURCE SYSTEMS CONTRIBUTIONS

* **Linux Kernel (`drivers/staging/iio/impedance-analyzer/ad5933.c`)** — *Kernel Locking Modernization*
  * Refactored multi-branch lock handling in the AD5933 impedance analyzer driver to scoped RAII `guard(mutex)` semantics, structurally preventing mutex leakage across error exits.
  * Formatted, checked (`checkpatch.pl --strict`), and submitted patches to subsystem maintainers via `git send-email`.
* **ValveSoftware/gamescope** — *Linux Display Compositor & Multi-Planar Memory*
  * Resolved Vulkan 1.3 NV12 storage image format verification and DRM modifier stride negotiation on NVIDIA proprietary drivers, restoring real-time frame streaming across PipeWire and NVENC.
* **ValveSoftware/Proton** — *Wine Compatibility Runtime & POSIX Filesystem*
  * Corrected DriverStore directory case-handling and file descriptor leakages to prevent Linux filesystem case collisions during Windows DLL resolution.
* **boykopovar/AnyPS5** — *PlayStation 5 Runtime Translation Layer*
  * Implemented `sceSysmoduleIsLoadedInternal` lifecycle state tracking (*merged upstream*), implemented FreeBSD `tgammaf` precision routines, and audited PRX stubs against automated Python AST header verification.

---

### FEATURED SYSTEMS & BACKEND PROJECTS

#### **mini-projects (Systems Lab)** — *In-Memory Key-Value Store & Distributed Primitives (C++17)*
* **Lock-Striped In-Memory Storage Engine:**
  * Engineered a multi-threaded in-memory key-value database in C++17 featuring a custom text protocol, passive retrieval expiration, and active background sweep threads.
  * Implemented **64-shard lock-striped hash partitioning** ([ADR 0001](https://github.com/ErenYeager1836/mini-projects/blob/main/kv-store/docs/adr/0001-lock-striping-vs-global-mutex.md)), eliminating global mutex lock convoying.
  * Benchmarked via Google Benchmark: sustained **`425,000+ ops/sec`** at **`< 0.18ms p99 latency`** under 16 concurrent threads.
  * Designed atomic Append-Only File (AOF) snapshotting utilizing length-prefixed serialization and filesystem rename primitives (`fs::rename`) for crash consistency.
* **High-Throughput Rate Limiting & Task Queue:**
  * Implemented sliding-window rate limiters with millisecond timestamp precision and an asynchronous worker pool handling concurrent tasks.
* **Memory Safety & CI Pipeline:**
  * Validated zero memory leaks (`0 bytes in 0 blocks lost`) under Valgrind Memcheck.
  * Built an automated GitHub Actions CI matrix validating builds across **Ubuntu, Windows, and macOS** with AddressSanitizer (ASan) and UndefinedBehaviorSanitizer (UBSan).

#### **secret-scanner** — *High-Throughput Entropy Scanning Engine*
* Implemented mathematical Shannon entropy heuristics ($H \ge 3.2$) across sliding character buffers to detect cryptographic secrets.
* Scanned Git commit DAG histories at **`~85 MB/s`** with `< 42MB` memory footprint, ensuring full output masking.

---

### EDUCATION
* **Bachelor of Engineering (B.E.) in Materials Science and Engineering**  
  College of Engineering, Guindy (CEG), Anna University, Chennai
