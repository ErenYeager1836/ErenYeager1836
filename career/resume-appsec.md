# Nithishvelan M
**Application Security & DevSecOps Engineer**  
Chennai, India | [mnithishvelan@gmail.com](mailto:mnithishvelan@gmail.com) | [github.com/ErenYeager1836](https://github.com/ErenYeager1836) | [linkedin.com/in/nithishvelan-m-096847263](https://www.linkedin.com/in/nithishvelan-m-096847263/)

---

### PROFESSIONAL SUMMARY
Application Security & Systems Engineer specializing in Static Application Security Testing (SAST), Abstract Syntax Tree (AST) analysis, security telemetry (OASIS SARIF 2.1.0), and automated vulnerability gating. Upstream open-source contributor to the Linux Kernel and security advisory databases. Experienced in developing custom security scanning engines, Shannon entropy credential detectors, and DevSecOps CI/CD pipelines.

---

### CORE TECHNICAL SKILLS
* **Security & SAST:** Static Application Security Testing (SAST), AST Traversal, OASIS SARIF 2.1.0, Threat Modeling (STRIDE), Shannon Entropy Heuristics, OWASP Top 10, CWE Mapping, Zero-Leakage Data Masking.
* **Languages & Systems:** Python 3, C++17/20, C, Bash/POSIX Shell, SQL, Git Internals.
* **DevSecOps & Tooling:** GitHub Actions CI/CD, Docker, Pre-Commit Framework, Flake8, Pytest, Valgrind, Linux Security Modules.

---

### UPSTREAM OPEN SOURCE SECURITY CONTRIBUTIONS
* **Linux Kernel (`drivers/staging/iio/ad5933.c`)** — *Kernel Concurrency & Lock Modernization*
  * Refactored multi-branch error-path mutex handling in the AD5933 impedance analyzer driver to use RAII `guard(mutex)` (`<linux/cleanup.h>`), structurally eliminating lock-leakage deadlocks.
  * Formatted and submitted patches adhering strictly to LKML conventions via `git send-email` to upstream subsystem maintainers (Jonathan Cameron, Greg Kroah-Hartman).
* **GitHub Advisory Database (`github/advisory-database`)** — *Security Advisory Research*
  * Audited and contributed vulnerability scope documentation improvements and metadata clarification for published CVE advisories.
* **boykopovar/AnyPS5** — *Static Analysis & Header Invariant Enforcement*
  * Resolved duplicate symbol declaration failures detected by Python AST static analysis (`public_headers`), ensuring single-source-of-truth export hygiene across 12,000+ virtual ABI symbols.

---

### FEATURED SECURITY PROJECTS

#### **ast-guard** — *AST-Based Static Application Security Testing Engine & GitHub Action*
* Engineered a lightweight Python AST SAST scanner analyzing code structures via `ast.NodeVisitor` with **0% comment/docstring false-positive rates** compared to regex linters.
* Emits native **OASIS SARIF 2.1.0** telemetry, enabling automated security alerts within GitHub Code Scanning and PR annotation gates.
* Benchmarked against 100,000+ LOC codebases; executes full security rule evaluation in **`< 1.2 seconds`** with `< 48MB` RSS memory overhead.
* Published as a reusable Composite **GitHub Action** (`ErenYeager1836/ast-guard@v1`) with configurable severity gates (`--fail-on HIGH`).
* Authored **ADR 0001: AST Visitor Traversal vs. Regular Expression Pattern Matching**.

#### **secret-scanner** — *High-Entropy Credential Detection CLI & Git History Auditor*
* Built an automated secret scanner implementing **Shannon entropy heuristics** ($H \ge 3.2$) alongside regex pattern matching to detect unpatterned API tokens and cryptographic keys.
* Implemented full Git DAG history inspection (`git log -p`), detecting leaked credentials retroactively deleted in subsequent commits.
* Enforced **guaranteed dynamic masking** to prevent cleartext secret leakage across stdout and JSON output artifacts.
* Benchmarked sustained filesystem scan throughput at **`~85 MB/s`** with `< 42MB` memory footprint.

#### **reconflow** — *Modular Security Reconnaissance CLI (Published on PyPI)*
* Developed a multi-threaded reconnaissance tool orchestrating passive DNS enumeration, subdomain discovery, and live HTTP probing.
* Selected and indexed by the **XERJ Corpus Hub** as an open-source reference implementation for AI coding agents.

#### **security-labs** — *Defensive Security & Vulnerability Simulation Suite*
* Developed socket-level port scanners, authentication log analyzers, and automated Linux host hardening auditing scripts.
* Configured an intentionally vulnerable multi-tier test lab in Docker demonstrating SQLi, IDOR, and privilege escalation defenses.

---

### EDUCATION
* **Bachelor of Engineering (B.E.) in Materials Science and Engineering**  
  College of Engineering, Guindy (CEG), Anna University, Chennai
