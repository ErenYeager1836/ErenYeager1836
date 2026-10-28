# Nithishvelan M
**Game Systems & Engine Programmer**  
Chennai, India | [mnithishvelan@gmail.com](mailto:mnithishvelan@gmail.com) | [github.com/ErenYeager1836](https://github.com/ErenYeager1836) | [linkedin.com/in/nithishvelan-m-096847263](https://www.linkedin.com/in/nithishvelan-m-096847263/)

---

### PROFESSIONAL SUMMARY
Game Systems & Low-Level Programmer specializing in modern C++17/20, Data-Oriented Design (DoD), cache-friendly memory architectures (Sparse Sets), real-time 2D physics simulation, and graphics compositing. Open-source contributor to Valve’s Linux gaming infrastructure (`gamescope`, `Proton`) and PlayStation 5 translation tooling (`AnyPS5`). Experienced in designing deterministic game loops, SAT collision solvers, and low-latency rendering pipelines.

---

### CORE TECHNICAL SKILLS
* **Languages & Standards:** C++17, C++20, C, Python 3, POSIX Shell, CMake.
* **Game Engine Architecture:** Data-Oriented Design (DoD), Entity Component System (ECS), Sparse Sets, Cache Line Saturation, Fixed-Timestep Accumulators, State Machines (FSM).
* **Physics & Math:** Separating Axis Theorem (SAT), Impulse-Based Collision Response, Minimum Translation Vectors (MTV), A* Pathfinding (Manhattan/Octile), Procedural Dungeon Generation (BSP).
* **Graphics & Systems:** Vulkan 1.3, PipeWire, POSIX Sockets, Multi-threading, AddressSanitizer (ASan), UndefinedBehaviorSanitizer (UBSan), Valgrind.

---

### UPSTREAM OPEN SOURCE GAME & COMPATIBILITY CONTRIBUTIONS

* **ValveSoftware/gamescope** — *Linux Gaming Session Compositor & Vulkan Pipeline*
  * Diagnosed and resolved video corruption during Steam Remote Play sessions on NVIDIA proprietary drivers by introducing multi-planar capability checks on NV12 storage images (`VK_IMAGE_USAGE_STORAGE_BIT`).
  * Addressed DRM format modifier stride alignment issues when exchanging video frames across PipeWire and NVENC hardware encoders.
* **ValveSoftware/Proton** — *Steam Play Windows Compatibility Tool (Wine)*
  * Resolved Linux case-sensitivity collisions during Windows DLL resolution by correcting DriverStore directory path lookups and file descriptor lifecycle management.
* **boykopovar/AnyPS5** — *PlayStation 5 Systems PRX Translation Layer*
  * Implemented `sceSysmoduleIsLoadedInternal` lifecycle state tracking (*merged upstream*), implemented FreeBSD `tgammaf` precision routines, and audited PRX exports against upstream Python AST header integrity tests.

---

### FEATURED GAME ENGINE & SYSTEMS PROJECTS

#### **game-dev-labs** — *Data-Oriented Game Engine Architecture Suite (C++17)*
* **Sparse Set Entity Component System (ECS):**
  * Built an ECS utilizing the **Sparse Set** data structure, guaranteeing $O(1)$ component addition, lookup, and swap-and-pop deletion while maintaining 100% cache-line saturation.
  * Verified $O(1)$ defragmentation with zero virtual dynamic dispatch overhead in simulation loops; processes **50,000 entities in `< 0.82ms`**.
  * Authored **ADR 0001: Sparse Sets vs. Contiguous Dense Arrays in ECS**.
* **2D Physics Engine (SAT Rigid Body Dynamics):**
  * Implemented the **Separating Axis Theorem (SAT)** for arbitrary convex polygons and circles, extracting Minimum Translation Vectors (MTV) for contact manifolds.
  * Engineered an impulse-based momentum and angular rotation solver incorporating Coulomb friction and positional slop projection to prevent object sinking.
* **Procedural Generation & A\* Pathfinding:**
  * Implemented Binary Space Partitioning (BSP) dungeon generation with $100\%$ topological room connectivity and 4-5 Cellular Automata cave smoothing.
  * Built an optimized $A^*$ navigation solver with Octile/Manhattan distance heuristics and priority queue tie-breaking.
* **Deterministic Game Loop & FSM:**
  * Implemented Glenn Fiedler’s fixed-timestep accumulator with state interpolation alpha ($\alpha$) to decouple physics execution from variable refresh rates.

#### **AnyPS5-Launcher** — *Modern Desktop Launcher & Reverse Engineering Hub*
* Developed a desktop launcher and executable analyzer for PS5 homebrew/relinked binaries, providing automated ELF header validation, PRX dependency resolution, and runtime controller mapping.

---

### ENGINEERING DISCIPLINE & RELIABILITY
* **Sanitizer-Driven CI:** All C++ simulation code is validated on GitHub Actions across **Ubuntu and Windows** under **AddressSanitizer (ASan)** and **UndefinedBehaviorSanitizer (UBSan)**.
* **Memory Integrity:** Validated zero memory leaks using Valgrind Memcheck (`0 bytes lost in 0 blocks`).

---

### EDUCATION
* **Bachelor of Engineering (B.E.) in Materials Science and Engineering**  
  College of Engineering, Guindy (CEG), Anna University, Chennai
