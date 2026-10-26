# SystemBoost Pro — Architecture Documentation

## 1. System Overview
**SystemBoost Pro** is a Windows desktop utility built on **.NET 10** and **Windows Presentation Foundation (WPF)**. It is designed with strict separation between UI presentation, application orchestration, domain logic, and Windows OS infrastructure so that all operations are auditable, reversible where applicable, and testable without modifying the host system during automated tests or CI execution.

---

## 2. Target Solution & Layered Architecture

```text
BoostSpeed/
├── .github/
│   └── workflows/
│       └── ci.yml                 # Windows + .NET 10 CI build & test pipeline
├── docs/
│   ├── ARCHITECTURE.md            # System & supervision architecture
│   ├── PROGRESS.md                # Milestone tracking & review status
│   ├── QA_REPORT.md               # Test & verification reports
│   └── REVIEW_REQUEST.md          # Structured review package for ChatGPT & PO
├── src/                           # (Planned in Milestone 1+)
│   ├── SystemBoostPro.App/        # WPF UI Layer (Views, ViewModels, Themes)
│   ├── SystemBoostPro.Core/       # Domain Models, Interfaces, Safety Policies
│   └── SystemBoostPro.Infrastructure/ # Windows OS Providers (Guarded by Dry-Run/Safe Abstractions)
├── tests/                         # (Planned in Milestone 1+)
│   └── SystemBoostPro.Tests/      # Unit & Integration Tests (Mocked/Sandboxed OS Layer)
├── .gitattributes                 # Line-ending normalization
├── .gitignore                     # .NET 10 & WPF ignore rules
└── AGENTS.md                      # AI Development & Supervision Governance
```

### 2.1 Layer Responsibilities
1. **Presentation Layer (`SystemBoostPro.App` — WPF / MVVM)**
   * Targets `net10.0-windows`.
   * Uses the MVVM pattern (`ObservableObject`, `RelayCommand`) with clean data binding and no destructive logic in code-behind.
2. **Core / Domain Layer (`SystemBoostPro.Core`)**
   * Targets `net10.0`.
   * Defines domain abstractions (`ISystemScanner`, `ICleanupEngine`, `IStartupManager`, `ISafetyGuard`), scan results, and risk classifications.
3. **Infrastructure Layer (`SystemBoostPro.Infrastructure`)**
   * Encapsulates file system, registry, and Windows service queries behind interfaces.
   * Enforces a mandatory `DryRun` / `SafeMode` guard when `SYSTEMBOOST_SAFE_CI=true` or during test execution, preventing any modification to host Windows settings or user files.
4. **Automated Test Suite (`SystemBoostPro.Tests`)**
   * Executes against in-memory fakes and temporary sandboxed directories only.
   * Never invokes destructive cleanup or Windows configuration changes.

---

## 3. Supervision & CI/CD Workflow Architecture

```text
[Lead Developer: Google Antigravity]
        │
        ▼ (Creates/updates feature/* branch)
[Local Git Repository]
        │
        ▼ (Push to GitHub remote after PO authentication)
[GitHub Actions CI (windows-latest, .NET 10 SDK)]
   ├── Checkout Repository
   ├── Setup .NET 10 SDK
   ├── Restore Dependencies (dotnet restore)
   ├── Build Solution (dotnet build)
   ├── Execute Available Tests (dotnet test)
   └── Publish CI Summary & TRX Artifacts (No Deploy / No OS Changes)
        │
        ▼ (Pull Request + docs/REVIEW_REQUEST.md)
[Supervisor Review: ChatGPT] ──► [Product Owner Confirmation] ──► [Merge to main]
```

### 3.1 Safety Guardrails in CI
* **Read-Only Permissions:** Workflow runs with `contents: read`.
* **Environment Variable Guard:** Sets `SYSTEMBOOST_SAFE_CI: 'true'`.
* **No Side Effects:** The CI pipeline only restores, builds, tests, and reports status. It never deploys artifacts, runs system cleanup utilities, or modifies Windows settings.
