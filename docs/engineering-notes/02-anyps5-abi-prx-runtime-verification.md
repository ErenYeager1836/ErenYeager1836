# Case Study: PS5 PRX ABI Emulation & Enforcing Static Header Invariants

**Author:** Nithishvelan M ([@ErenYeager1836](https://github.com/ErenYeager1836))  
**Target Subsystem:** [boykopovar/AnyPS5](https://github.com/boykopovar/AnyPS5)  
**Technologies:** C++20, PlayStation 5 System PRX, Virtual ABI (`APS5_VABI`), Python AST Static Analysis, ThreadSanitizer  

---

## 1. Executive Summary

This engineering note examines the architectural discipline required when implementing system software compatibility layers for proprietary platforms. Specifically, it reviews the handling of debugging modules (`libSceDbgThreadSanitizer`) in AnyPS5, the design philosophy behind **"Fail-Loud" throwing stubs vs. silent no-ops**, and the static analysis enforcement mechanism (`public_headers`) used to preserve ABI integrity.

---

## 2. Background: AnyPS5 System Architecture

AnyPS5 executes PlayStation 5 binaries natively on Linux and Windows without hardware emulation. It achieves this by:
1. **Binary Relinking:** Rewriting PS5 ELF executables into native host PE/ELF binaries.
2. **PRX Reimplementation:** Providing host-side C++ implementations of Sony's system libraries (`libkernel`, `libSceVideoOut`, `libSceNet`, `libSceDbgThreadSanitizer`, etc.).
3. **Virtual ABI (`APS5_VABI`):** Exporting functions through unified function attributes and Name Identification Hashes (NIDs) that map 1:1 with PS5 dynamic library imports.

---

## 3. The Design Dilemma: Throwing Stubs vs. Silent No-Ops

When emulating platform SDKs, developers frequently encounter developer-mode or diagnostic functions that have no direct host equivalent. 

In `libSceDbgThreadSanitizer`, two functions are imported by system modules:
* `__tsan_acquire(void* addr)`
* `__tsan_release(void* addr)`

### The Anti-Pattern: Speculative Silent Stubs (No-Ops)
A naive approach is to provide empty no-op implementations:
```cpp
// Flawed Approach:
void APS5_VABI __tsan_acquire(void* addr) { (void)addr; }
void APS5_VABI __tsan_release(void* addr) { (void)addr; }
```
**Why this is dangerous in systems engineering:**
* **Masking Fatal Divergence:** If a game title legitimately depends on side-effects (e.g., synchronization barriers or error state mutation), a silent no-op prevents early detection, causing downstream memory corruption or hard-to-debug deadlocks hundreds of frames later.
* **Violating Upstream Contracts:** In AnyPS5, PS5 titles only call these functions if `sceKernelIsThreadSanitizerEnabled()` returns non-zero. Since AnyPS5 reports thread sanitization as disabled, genuine titles never reach these routines.

### The Correct Architectural Pattern: Fail-Loud Throwing Stubs
Unreachable or unverified paths must explicitly terminate with structured error metadata:
```cpp
void APS5_VABI __tsan_acquire(void*) {
    NotImplemented_nid_no_patch(__func__);
}
```
If an unexpected game or module ever reaches this export, the process immediately aborts with an intelligible diagnostic trace, allowing developers to capture the exact game call stack and investigate the true requirements.

---

## 4. Header Hygiene & The `public_headers` Python AST Validator

To prevent header pollution and symbol collisions across hundreds of PRX modules, AnyPS5 enforces the following invariant:

> **Every exported function marked with `APS5_VABI` must possess exactly one declaration in a public header reachable through the library's root `Api.hpp`. Duplicate declarations in implementation files, headers, or unit tests are strictly forbidden.**

### The Static Analysis Mechanism
AnyPS5 enforces this rule in CI via `core/libs/tests/test_public_headers.py`, which parses the codebase using Python regex and C++ token stream analysis:
```python
DECLARATION = re.compile(
    r"(?:\bAPS5_VABI\s+|^[ \t]*[A-Za-z_][\w:*&<> \t]*?[ \t]+)(\w+)\s*\([^;{}]*\)\s*(?:noexcept\s*)?;", 
    re.M
)
```

### The Bug & Fix
When writing guest test harnesses (`GuestThreadSanitizer.cpp`), manually forward-declaring the functions:
```cpp
// FAILS public_headers validation:
extern "C" {
void APS5_VABI __tsan_acquire(void*);
void APS5_VABI __tsan_release(void*);
}
```
triggers duplicate declaration errors in static analysis:
```
RuntimeError: __tsan_release: expected exactly one header declaration; found 2:
  core/libs/prx/libSceDbgThreadSanitizer/include/Api.hpp:8
  core/libs/tests/GuestThreadSanitizer.cpp:7
```

**The Resolution:**
Remove manual declarations and include the module's umbrella header directly:
```cpp
#include "prx/libSceDbgThreadSanitizer/include/Api.hpp"
```
This preserves single-source-of-truth declarations and passes the automated 12,000+ export symbol integrity check:
```
368 libraries, 12228 exports: unique declarations and umbrella coverage verified
```

---

## 5. Engineering Takeaways

1. **Explicit Errors Over Silent Assumptions:** Never provide fake-success or empty stubs in system emulation unless backed by verified hardware/game traces.
2. **Automated Structural Invariants:** Large codebases (12,000+ exports) cannot be audited manually; static analysis AST tools in CI are required to enforce architectural cleanliness.
3. **Rigorous Test Discipline:** Tests must consume library APIs through the exact public header interfaces that consumer applications use.
