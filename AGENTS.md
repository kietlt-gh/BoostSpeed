# AI Development & Supervision Governance (`AGENTS.md`)

## 1. Overview
**SystemBoost Pro** is developed using a structured, GitHub-based AI development supervision workflow. Every milestone is implemented on an isolated feature branch, verified through automated CI and QA reporting, and submitted via Pull Request for review before merging into `main`.

---

## 2. Roles & Responsibilities

| Role | Agent / Entity | Responsibilities |
| :--- | :--- | :--- |
| **Product Owner** | Human User | Defines product vision and requirements, manages GitHub authentication/credentials, coordinates reviews with ChatGPT, confirms review approvals, and controls merges into `main`. |
| **Lead Developer** | Google Antigravity | Designs, implements, tests, and documents features and infrastructure on feature branches; maintains `docs/PROGRESS.md`, `docs/QA_REPORT.md`, `docs/ARCHITECTURE.md`, and `docs/REVIEW_REQUEST.md`. |
| **Supervisor / Reviewer** | ChatGPT | Reviews code changes, architecture decisions, test coverage, CI results, and `docs/REVIEW_REQUEST.md` prior to milestone sign-off. |

---

## 3. Mandatory Operational Rules

### Rule 1: Branching & Pull Request Discipline
* All development work **must** occur on dedicated branches (`feature/*`, `fix/*`, `chore/*`).
* **Never** commit directly to `main` or overwrite existing repository history.
* **Never** automatically merge branches into `main`. Merging is performed only after explicit Product Owner authorization.

### Rule 2: Security & Credential Protection
* **Never** print, log, expose, or commit credentials, Personal Access Tokens (PATs), SSH keys, certificates, or secrets.
* When GitHub authentication is required, pause and request the Product Owner to complete authentication securely.

### Rule 3: Review Integrity & Truthful Reporting
* **Never** claim or imply that ChatGPT has reviewed or approved code unless the Product Owner explicitly confirms it.
* **Never** report a passing Phase 0 infrastructure workflow as proof that application compilation or automated unit tests succeeded.
* Every milestone must update `docs/REVIEW_REQUEST.md` with:
  1. Completed work
  2. Changed files
  3. Test results (explicitly distinguishing Infrastructure Validation, Application Compilation, and Automated Test Execution)
  4. Known issues
  5. Remaining tasks
  6. Evidence of successful execution

### Rule 4: Milestone Documentation Synchronization
* After every development milestone or supervisor review iteration, the Lead Developer must synchronize:
  * `docs/PROGRESS.md`
  * `docs/QA_REPORT.md`
  * `docs/REVIEW_REQUEST.md`
  * `docs/ARCHITECTURE.md` (when architectural or safety policies are introduced or modified)

### Rule 5: CI/CD Validation & Defense-in-Depth Safety Constraints
* The GitHub Actions CI pipeline (`windows-latest`, `.NET 10 SDK`, Node 24 actions) must clearly distinguish between:
  1. **Infrastructure Validation** (`.NET 10 SDK` availability, supervision files, and explicit solution/project path enforcement once `.NET` projects are introduced)
  2. **Application Compilation** (`dotnet restore SystemBoostPro.sln` and `dotnet build SystemBoostPro.sln`)
  3. **Automated Test Execution** (`dotnet test SystemBoostPro.sln` with required `.trx` output)
* When the `.NET` solution is introduced in Phase 1, CI **must fail** if the explicit solution (`SystemBoostPro.sln`) or any required project (`SystemBoostPro.App`, `SystemBoostPro.Core`, `SystemBoostPro.Infrastructure`, `SystemBoostPro.Tests`) or test output is missing.
* All future system-modification functionality must enforce the **4-Layer Defense-in-Depth Safety Architecture** defined in `docs/ARCHITECTURE.md`:
  * Environment variable `SYSTEMBOOST_SAFE_CI=true` is only the outer CI boundary and **must never** be relied upon as the sole protection.
  * Code must enforce OS provider interfaces, runtime `ISafetyPolicyGuard` checks (default `DryRun` mode, CI/testhost detection, canonical path allowlists/denylists, and user confirmation tokens), and isolated in-memory or temporary sandbox test fixtures.
* The CI pipeline and automated tests **must never**:
  * Deploy binaries or releases automatically
  * Execute real system cleanup or file deletion outside isolated test temp sandboxes
  * Modify Windows Registry keys, services, startup tasks, or OS settings
