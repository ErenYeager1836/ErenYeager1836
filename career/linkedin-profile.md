# Complete LinkedIn Profile Copy

Copy and paste these exact sections into your LinkedIn profile to maximize recruiter visibility across **Systems Engineering**, **Application Security (AppSec)**, and **Game Engine / Low-Level C++** roles.

---

## 1. Headline (Within 220-Character Limit)

```text
Systems & Application Security Engineer | C++17/20, Linux Kernel, Vulkan, SAST | Upstream Contributor to Valve (Gamescope, Proton) & Linux Kernel | CEG, Anna University
```

---

## 2. About Section

```markdown
I am a Systems and Application Security Engineer specializing in low-level modern C++17/20, Linux kernel subsystems, binary ABI emulation, and Static Application Security Testing (SAST). 

Originally trained in Materials Science & Engineering at the College of Engineering, Guindy (CEG), Anna University, I bring a first-principles, hardware-aware mindset to software architecture—focusing on memory bus dynamics, cache-line saturation, concurrency safety, and compiler-level AST analysis.

🚀 Upstream Open-Source Impact:
• ValveSoftware/gamescope: Diagnosed Vulkan 1.3 NV12 multi-planar storage image validation and DRM format modifier stride alignment on NVIDIA proprietary drivers, resolving video stream corruption during Steam Remote Play.
• ValveSoftware/Proton: Resolved Linux filesystem case-sensitivity collisions in DriverStore path lookups and file descriptor lifecycles for Windows binary translation.
• Linux Kernel (Staging/IIO): Refactored multi-branch error-path lock handling in the AD5933 impedance analyzer driver to scoped RAII guard(mutex) semantics (<linux/cleanup.h>), eliminating lock-leakage deadlocks.
• boykopovar/AnyPS5: Implemented runtime module lifecycle tracking (sceSysmoduleIsLoadedInternal - merged upstream) and audited 12,000+ PRX exports against strict automated Python AST header invariants.

🛠️ Core Engineered Systems:
• ast-guard: Lightweight Python AST SAST scanner emitting native OASIS SARIF 2.1.0 telemetry for GitHub Code Scanning; scans 100k+ LOC in <1.2s with zero regex false positives (published as a GitHub Action: ErenYeager1836/ast-guard@v1).
• In-Memory Key-Value Store: 64-shard lock-striped in-memory database in C++17; sustained 425,000+ ops/sec at <0.18ms p99 latency across 16 threads with zero memory leaks verified under Valgrind, ASan, and TSan.
• game-dev-labs: Data-Oriented Design (DoD) C++17 engine suite featuring a Sparse Set Entity Component System (ECS) processing 50k entities in <0.82ms, SAT 2D physics solver, and deterministic fixed-timestep game loops.
• secret-scanner: High-throughput credential auditor using Shannon entropy heuristics (H >= 3.2) across Git DAG commit histories at ~85 MB/s with full output masking.

Technical Core: Modern C++17/20, C, Python 3, Linux Kernel Subsystems, Concurrency (pthreads, shared_mutex, lock striping), Vulkan 1.3, POSIX Sockets, Static Analysis (AST, SARIF 2.1.0), Threat Modeling, Sanitizers (ASan/UBSan/TSan), CMake, Docker.

Open to Systems Software Engineering, Application Security (AppSec), and Game Engine / Graphics roles.
📧 Contact: mnithishvelan@gmail.com
💻 GitHub: https://github.com/ErenYeager1836
```

---

## 3. Experience Section

### Position 1: Open Source Systems & Security Engineer
* **Company:** Independent Open Source Contributor (Valve, Linux Kernel, AnyPS5)
* **Location:** Remote
* **Dates:** [Your Start Date] – Present
* **Description:**
```text
Contribute low-level bug fixes, concurrency modernizations, and ABI compatibility routines to premier open-source repositories and Linux gaming infrastructure.

Key Contributions:
• Valve / Gamescope: Resolved Vulkan 1.3 NV12 multi-planar storage image validation failures on NVIDIA proprietary drivers; aligned DRM format modifier plane layouts across PipeWire and NVENC encoders to eliminate video stream corruption.
• Valve / Proton: Remediated DriverStore directory case-handling collisions and file descriptor leaks under Wine on Linux.
• Linux Kernel: Modernized lock handling in drivers/staging/iio/impedance-analyzer/ad5933.c by implementing scoped RAII guard(mutex) cleanup semantics, structurally eliminating error-path lock leaks.
• AnyPS5: Implemented PlayStation 5 system module lifecycle tracking (sceSysmoduleIsLoadedInternal - merged) and resolved duplicate symbol declarations detected by Python AST static analysis (public_headers) across 12,000+ exports.
```

---

## 4. Projects Section (For LinkedIn's "Projects" Feature)

### Project 1: ast-guard — AST-Based SAST Engine & GitHub Action
* **URL:** `https://github.com/ErenYeager1836/ast-guard`
* **Description:**
```text
Engineered an Abstract Syntax Tree (AST) static application security testing engine in Python emitting native OASIS SARIF 2.1.0 reports for GitHub Code Scanning. 
• Eliminates comment and docstring false positives inherent to regex pattern matchers.
• Benchmarked at 100k+ LOC scanned in <1.2 seconds with <48MB RSS memory.
• Packaged as a composite GitHub Action (ErenYeager1836/ast-guard@v1) for automated PR security gates.
```

### Project 2: High-Concurrency In-Memory Key-Value Store (C++17)
* **URL:** `https://github.com/ErenYeager1836/mini-projects/tree/main/kv-store`
* **Description:**
```text
Architected a multi-threaded in-memory key-value database in C++17 inspired by Redis.
• Implemented 64-shard lock-striped hash partitioning, eliminating global mutex contention.
• Sustains 425,000+ ops/sec at <0.18ms p99 tail latency under 16 concurrent worker threads.
• Implemented atomic Append-Only File (AOF) snapshotting and verified zero memory leaks under AddressSanitizer (ASan), ThreadSanitizer (TSan), and Valgrind Memcheck.
```

### Project 3: game-dev-labs — Data-Oriented Game Engine Architecture Suite
* **URL:** `https://github.com/ErenYeager1836/game-dev-labs`
* **Description:**
```text
Engineered a C++17 modular game engine architecture demonstrating cache-friendly memory layouts and deterministic simulation.
• Built a Sparse Set Entity Component System (ECS) delivering O(1) addition, lookup, and swap-and-pop deletion with 100% cache line saturation; updates 50,000 entities in <0.82ms.
• Built a 2D physics engine using the Separating Axis Theorem (SAT), Minimum Translation Vectors (MTV), and impulse momentum resolution.
• Implemented Glenn Fiedler's fixed-timestep accumulator with state interpolation alpha for frame-rate-independent rendering.
```

---

## 5. Education Section

* **School:** College of Engineering, Guindy (CEG), Anna University, Chennai
* **Degree:** Bachelor of Engineering - BE
* **Field of Study:** Materials Science and Engineering
* **Activities & Societies:** Technical Computing, Computational Physics, Systems Architecture
* **Description:**
```text
Rigorous engineering and analytical training in physical systems modeling, continuum mechanics, computational thermodynamics, and numerical simulations. Applied foundational engineering principles directly to low-level systems programming, cache-line memory dynamics, and concurrency.
```

---

## 6. Skills Section (Top 20 Keywords for Recruiter Search)

Add these to your profile's Skills section (and pin the top 3):

**Top 3 Pinned Skills:**
1. **C++ (C++17, C++20)**
2. **Application Security (AppSec)**
3. **Linux Kernel**

**Remaining Technical Skills:**
* Static Application Security Testing (SAST)
* Vulkan
* Concurrency & Multi-threading
* Systems Programming
* Threat Modeling
* Data-Oriented Design (DoD)
* SARIF
* Memory Management
* POSIX Sockets
* Valgrind / Dynamic Sanitizers (ASan, TSan)
* Git Internals
* CMake
* Python
* Docker
* DevSecOps
* Shannon Entropy Analysis
* Reverse Engineering
