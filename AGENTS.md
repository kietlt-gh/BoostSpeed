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

### Rule 3: Review Integrity
* **Never** claim or imply that ChatGPT has reviewed or approved code unless the Product Owner explicitly confirms it.
* Every milestone must update `docs/REVIEW_REQUEST.md` with:
  1. Completed work
  2. Changed files
  3. Test results
  4. Known issues
  5. Remaining tasks
  6. Evidence of successful execution

### Rule 4: Milestone Documentation Synchronization
* After every development milestone, the Lead Developer must update:
  * `docs/PROGRESS.md`
  * `docs/QA_REPORT.md`
  * `docs/REVIEW_REQUEST.md`
  * `docs/ARCHITECTURE.md` (when architectural components are introduced or modified)

### Rule 5: CI/CD & System Safety Constraints
* The GitHub Actions CI pipeline (`windows-latest`, `.NET 10 SDK`) is strictly restricted to:
  1. Restoring NuGet dependencies (`dotnet restore`)
  2. Building the solution (`dotnet build`)
  3. Running automated unit/integration tests (`dotnet test`)
  4. Reporting build and test outcomes
* The CI pipeline and automated tests **must never**:
  * Deploy binaries or releases automatically
  * Execute real system cleanup or file deletion outside isolated test temp directories
  * Modify Windows Registry keys, services, startup tasks, or OS settings
