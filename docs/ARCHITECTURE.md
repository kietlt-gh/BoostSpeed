# SystemBoost Pro — Architecture Documentation (`ARCHITECTURE.md`)

## 1. System Overview
**SystemBoost Pro** is a Windows desktop utility built on **.NET 10** and **Windows Presentation Foundation (WPF)**. It is designed with strict separation between UI presentation, application orchestration, domain logic, and Windows OS infrastructure so that all operations are auditable, reversible where applicable, and testable without modifying the host system during automated tests or CI execution.

---

## 2. Explicit Solution & Layered Project Structure

Once Phase 1 application development begins, the repository will use the following explicit solution and project paths (enforced by `.github/workflows/ci.yml`):

```text
BoostSpeed/
├── .github/
│   └── workflows/
│       └── ci.yml                                           # Windows + .NET 10 CI pipeline
├── docs/
│   ├── ARCHITECTURE.md                                      # System & safety architecture
│   ├── PROGRESS.md                                          # Milestone tracking & review status
│   ├── QA_REPORT.md                                         # Test & verification reports
│   └── REVIEW_REQUEST.md                                    # Structured review package for ChatGPT & PO
├── src/                                                     # (Phase 1+ Application Source)
│   ├── SystemBoostPro.App/
│   │   └── SystemBoostPro.App.csproj                        # WPF UI Layer (net10.0-windows)
│   ├── SystemBoostPro.Core/
│   │   └── SystemBoostPro.Core.csproj                       # Domain Models, Interfaces, Safety Guards (net10.0)
│   └── SystemBoostPro.Infrastructure/
│       └── SystemBoostPro.Infrastructure.csproj             # Windows OS Adapters guarded by ISafetyPolicyGuard
├── tests/                                                   # (Phase 1+ Automated Test Suite)
│   └── SystemBoostPro.Tests/
│       └── SystemBoostPro.Tests.csproj                      # Isolated Unit & Sandboxed Tests (net10.0)
├── SystemBoostPro.sln                                       # Explicit root solution file (Phase 1+)
├── .gitattributes                                           # Line-ending normalization
├── .gitignore                                               # .NET 10 & WPF ignore rules
└── AGENTS.md                                                # AI Development & Supervision Governance
```

### 2.1 Layer Responsibilities
1. **Presentation Layer (`src/SystemBoostPro.App/SystemBoostPro.App.csproj` — WPF / MVVM)**
   * Targets `net10.0-windows`.
   * Uses the MVVM pattern (`ObservableObject`, `RelayCommand`) with clean data binding and zero direct OS or file-system calls in code-behind or ViewModels.
2. **Core / Domain Layer (`src/SystemBoostPro.Core/SystemBoostPro.Core.csproj`)**
   * Targets `net10.0`.
   * Defines domain models, scan/cleanup contracts (`ISystemScanner`, `ICleanupEngine`, `IStartupManager`), and the mandatory runtime safety contract (`ISafetyPolicyGuard`, `SystemOperationMode`, `SafetyExecutionToken`).
   * **Prohibition:** Domain code must never invoke `System.IO.File`, `System.IO.Directory`, `Microsoft.Win32.Registry`, WMI, or `System.Diagnostics.Process` directly.
3. **Infrastructure Layer (`src/SystemBoostPro.Infrastructure/SystemBoostPro.Infrastructure.csproj`)**
   * Encapsulates file system, registry, and Windows service queries behind narrow adapter interfaces (`IFileSystemProvider`, `IRegistryProvider`, `IServiceControlProvider`).
   * Every mutating method requires mandatory pre-execution validation via `ISafetyPolicyGuard.AssertMutationAllowed(...)`.
4. **Automated Test Suite (`tests/SystemBoostPro.Tests/SystemBoostPro.Tests.csproj`)**
   * Executes exclusively against in-memory test doubles (`InMemoryFileSystemProvider`, `InMemoryRegistryProvider`) or strictly bounded temporary sandbox directories.
   * Never resolves or invokes live Windows configuration mutators.

---

## 3. Defense-in-Depth Safety Architecture

### 3.1 Why `SYSTEMBOOST_SAFE_CI` Alone Is Insufficient
Relying solely on the `SYSTEMBOOST_SAFE_CI=true` environment variable is **not** sufficient protection against accidental host modification because:
1. Developers or test runners executing `dotnet test` locally may not have `SYSTEMBOOST_SAFE_CI` set in their shell environment.
2. Environment variables can be stripped or overridden when spawning child processes.
3. Any code that directly calls static .NET OS APIs (e.g., `File.Delete`, `RegistryKey.DeleteValue`) without passing through an architectural guard would bypass an environment variable check altogether.

Therefore, **SystemBoost Pro** enforces a mandatory **4-Layer Defense-in-Depth Safety Model** for all future system-modification functionality:

```text
[Caller / Service Request]
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ LAYER 1: Architectural Abstraction Boundary                             │
│  • No direct calls to File/Directory/Registry/Process in Core or UI.    │
│  • All OS interactions routed through injected provider interfaces.     │
└─────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ LAYER 2: Runtime Safety Policy Guard (ISafetyPolicyGuard)               │
│  • Default Mode: SystemOperationMode.DryRun (read-only simulation).     │
│  • Blocks all mutations if ANY of the following are true:               │
│    1. Environment variable SYSTEMBOOST_SAFE_CI == "true"                │
│    2. Environment variable CI == "true" or GITHUB_ACTIONS == "true"     │
│    3. Process is hosted inside a test runner (testhost / vstest / xunit)│
│    4. Missing explicit interactive user SafetyExecutionToken            │
│    5. Target path is outside approved cleanup scopes or matches         │
│       the Protected Path Denylist (Windows, Program Files, repo, etc.)  │
│  • Throws UnsafeSystemOperationException on any violation.              │
└─────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ LAYER 3: Isolated Test Doubles & Sandboxed Fixtures                     │
│  • Unit tests inject InMemoryFileSystemProvider / FakeRegistryProvider. │
│  • File I/O integration tests use DisposableSandboxDirectory rooted in  │
│    %TEMP%\SystemBoostPro.Tests.Sandbox\<Guid> with strict path-prefix   │
│    canonicalization (GetFullPath) rejecting traversal or external paths.│
└─────────────────────────────────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────────────────────────────────┐
│ LAYER 4: CI Environment Enforcement (.github/workflows/ci.yml)          │
│  • Sets SYSTEMBOOST_SAFE_CI='true' and permissions 'contents: read'.    │
│  • Dedicated unit tests verify ISafetyPolicyGuard throws when           │
│    SYSTEMBOOST_SAFE_CI='true' or when running inside testhost.          │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Mandatory Safety Guard Contract (Phase 1+ Specification)
When system-modification capabilities are implemented in Phase 1+, `SystemBoostPro.Core` and `SystemBoostPro.Infrastructure` must implement and test these rules:
1. **Safe-by-Default Configuration (`SystemOperationMode.DryRun`):**
   * By default, all cleanup and optimization engines instantiate in `DryRun` mode, which calculates candidate actions and byte counts without altering disk or registry state.
2. **Environment & Host Process Detection (`ISafetyPolicyGuard`):**
   * `ISafetyPolicyGuard` inspects:
     * `Environment.GetEnvironmentVariable("SYSTEMBOOST_SAFE_CI")`
     * `Environment.GetEnvironmentVariable("CI")` and `Environment.GetEnvironmentVariable("GITHUB_ACTIONS")`
     * Current process/domain name (`AppDomain.CurrentDomain.FriendlyName` / `Environment.ProcessPath` matching `testhost`, `vstest`, `xunit`)
   * If any indicator is active, `IsLiveSystemMutationPermitted` is permanently `false` for the lifetime of the process, and any attempt to execute a non-sandboxed mutation throws `UnsafeSystemOperationException`.
3. **Canonical Path Allowlist & Denylist Validation:**
   * All file paths must be canonicalized via `Path.GetFullPath()` and checked against reparse points / symlinks (`FileAttributes.ReparsePoint`) before any operation.
   * System-critical roots (`%SystemRoot%`, `%ProgramFiles%`, `%ProgramFiles(x86)%`, `%USERPROFILE%\Documents`, `%USERPROFILE%\Desktop`, and the active repository root) are unconditionally denylisted.
4. **Isolated Test Requirement:**
   * Every service capable of modifying files, registry entries, startup items, or services must have unit tests verifying:
     1. Mutations are blocked with `UnsafeSystemOperationException` when `SYSTEMBOOST_SAFE_CI=true`.
     2. Mutations are blocked in `DryRun` mode.
     3. Out-of-scope or denylisted paths are rejected.

---

## 4. CI/CD Validation & Reporting Architecture

```text
[Push / Pull Request on GitHub]
        │
        ▼
[Stage 1: Infrastructure Validation]
  • Checkout (actions/checkout@v7 on Node 24)
  • Setup .NET 10 SDK (actions/setup-dotnet@v6 on Node 24)
  • Verify supervision governance files exist
  • Check solution/project structure:
      ├─► If no .sln/.csproj yet (Phase 0):
      │     Mark Application Compilation = NOT EXECUTED
      │     Mark Automated Test Execution = NOT EXECUTED
      └─► If any .sln/.csproj exists (Phase 1+):
            Require explicit SystemBoostPro.sln + App/Core/Infra/Tests .csproj
            (Fail CI immediately if any required project or test project is missing)
        │
        ▼ (Only when has_solution == 'true')
[Stage 2: Application Compilation]
  • dotnet restore SystemBoostPro.sln
  • dotnet build SystemBoostPro.sln --configuration Release --no-restore
        │
        ▼ (Only when has_solution == 'true')
[Stage 3: Automated Test Execution]
  • dotnet test SystemBoostPro.sln --configuration Release --no-build --logger trx
  • Require non-empty .trx output and upload via actions/upload-artifact@v7
        │
        ▼
[Stage 4: Validation Summary Report]
  • Distinctly reports:
      1. Infrastructure Validation (PASSED / FAILED)
      2. Application Compilation (PASSED / FAILED / NOT EXECUTED)
      3. Automated Test Execution (PASSED / FAILED / NOT EXECUTED)
```

### 4.1 Strict Separation of CI Reporting
* **Infrastructure Validation** only proves that the `.NET 10 SDK` runner environment and supervision governance files are valid.
* **Application Compilation** proves that `SystemBoostPro.sln` and its constituent projects restore and compile cleanly with zero build errors.
* **Automated Test Execution** proves that `SystemBoostPro.Tests` executed and passed with generated `.trx` evidence.
* A passing Phase 0 infrastructure run **must never** be reported as proof that application compilation or unit tests succeeded.
