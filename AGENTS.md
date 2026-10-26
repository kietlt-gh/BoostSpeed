# AI Development & Phase-Based Supervision Governance (`AGENTS.md`)

## 1. Overview
**SystemBoost Pro** is a professional Windows optimization application inspired by Auslogics BoostSpeed. Development follows a strict **Phase-Based AI Supervision** workflow where implementation, verification, and technical reporting are completed per phase and reviewed by an external AI Technical Supervisor before advancing to the next phase.

---

## 2. Team Roles & Responsibilities

| Role | Entity | Responsibilities |
| :--- | :--- | :--- |
| **Product Owner** | Human User | Defines requirements, reviews phase reports presented by the Lead Developer, coordinates reviews with ChatGPT, confirms supervisor approvals, and authorizes starting the next phase. |
| **Technical Supervisor** | ChatGPT | Reviews phase reports (`reports/PHASE_XX_REPORT.md` / `reports/LATEST_REPORT.md`) and verification evidence, identifies technical defects or risks, and recommends whether a development phase should be approved. |
| **Lead Developer** | Google Antigravity | Creates implementation plans, implements features, builds and tests the application, fixes defects, collects real verification evidence under `reports/evidence/`, generates comprehensive Vietnamese phase reports (`reports/PHASE_XX_REPORT.md` and `reports/LATEST_REPORT.md`), commits to Git, and stops after each phase to request supervisor review. |

---

## 3. Phase-Based Development Workflow

Continuous GitHub Pull Request / GitHub Actions supervision is **replaced by Phase-Based Technical Reporting**:
* **Do not depend on GitHub Actions for supervisor approval.**
* GitHub remains used for version control and source code backup.

Every development phase must follow this 10-step lifecycle:
1. Receive development requirements.
2. Create an implementation plan.
3. Implement the required functionality.
4. Build the application.
5. Execute relevant tests.
6. Fix discovered defects.
7. Verify implemented functionality.
8. Generate a comprehensive phase report (`reports/PHASE_XX_REPORT.md` and copy to `reports/LATEST_REPORT.md`).
9. Present the report to the Product Owner.
10. **Stop and request external supervisor review.** (Never automatically start the next major development phase.)

---

## 4. Reporting Directory & Naming Convention

All phase reports and verification artifacts live under `reports/`:

```text
reports/
├── PHASE_REPORT_TEMPLATE.md     # Standard 14-section Vietnamese report template
├── PHASE_01_REPORT.md           # Phase 01 dedicated technical report
├── PHASE_02_REPORT.md           # Phase 02 dedicated technical report (future)
├── PHASE_03_REPORT.md           # Phase 03 dedicated technical report (future)
├── LATEST_REPORT.md             # Complete copy of the latest phase report
└── evidence/                    # Supporting verification artifacts
    ├── *.log                    # Build logs, test summaries, runtime logs
    └── *.png                    # Real running UI screenshots (when applicable)
```

### Language & Quality Rules
* All phase reports **must be written in Vietnamese**.
* Technical terminology, class names, file names, CLI commands, and code references may remain in English.
* `reports/LATEST_REPORT.md` must always contain a complete, exact copy of the latest `reports/PHASE_XX_REPORT.md`.
* Avoid generic unverified claims (e.g., *"Everything works perfectly"*); always cite specific, verifiable evidence from `reports/evidence/`.

---

## 5. Mandatory 14-Section Report Structure

Every `reports/PHASE_XX_REPORT.md` and `reports/LATEST_REPORT.md` must contain all 14 sections in order:

1. **SECTION 1 — PROJECT INFORMATION:** Project name, current phase, report date, Git branch, latest commit SHA, development environment, .NET SDK version, operating system.
2. **SECTION 2 — PHASE OBJECTIVES:** Original requirements, expected deliverables, acceptance criteria, development scope.
3. **SECTION 3 — COMPLETED FEATURES:** Feature name, description, implementation status (`COMPLETED`, `PARTIAL`, `NOT IMPLEMENTED`, `BLOCKED`), main source files, testing status, verification evidence. Never mark `COMPLETED` without implementation evidence.
4. **SECTION 4 — SOURCE CODE CHANGES:** New files, modified files, removed files, important classes, important services, architectural changes, and actual file paths.
5. **SECTION 5 — BUILD VERIFICATION:** Exact build command, build environment, build result, error count, warning count, build log excerpts, and path to complete build log. If not executed, explicitly state **`BUILD NOT EXECUTED`**.
6. **SECTION 6 — TEST RESULTS:** Test categories, commands, total/passed/failed/skipped counts, integration test results, manual verification results. Never confuse compilation with runtime testing.
7. **SECTION 7 — UI VERIFICATION:** Screenshots of the running application saved in `reports/evidence/`, verified screens, visual defects, unfinished components, navigation behavior, theme switching. Never use mockups as runtime evidence.
8. **SECTION 8 — KNOWN ISSUES:** Issue description, severity (`CRITICAL`, `HIGH`, `MEDIUM`, `LOW`), affected component, reproduction steps, proposed solution, current status.
9. **SECTION 9 — SECURITY AND WINDOWS SAFETY:** Evaluation of administrator privileges, file deletion, registry modifications, Windows service modifications, system config changes, backup/recovery, user confirmation, error handling, and data loss risks. Never execute destructive maintenance during report generation.
10. **SECTION 10 — PERFORMANCE REVIEW:** Startup time, memory, CPU, UI responsiveness, background operation metrics and collection methodology. Never invent benchmark values; if not measured, state explicitly that performance was not measured.
11. **SECTION 11 — UNFINISHED WORK:** Incomplete features, missing requirements, technical limitations, deferred improvements, outstanding defects.
12. **SECTION 12 — SELF-ASSESSMENT:** Technical evaluation of code quality, architecture, functional completeness, reliability, security, maintainability, and test coverage, strictly separating verified facts from subjective assessments.
13. **SECTION 13 — NEXT PHASE RECOMMENDATIONS:** Recommended next tasks, dependencies, technical risks, suggested implementation order.
14. **SECTION 14 — SUPERVISOR REVIEW REQUEST:** Must conclude with **`SUPERVISOR REVIEW REQUIRED`**, listing features requiring review, unresolved issues, technical decisions, build/test evidence, and questions for ChatGPT.

---

## 6. Security, Privacy, and Windows System Safety Rules

1. **No Sensitive Data in Reports or Evidence:** Never include credentials, Personal Access Tokens, personal user files, or sensitive machine identifiers in `reports/`, `reports/evidence/`, or Git commits.
2. **Windows Host Protection:** Do not execute destructive system cleanup, file deletion, registry modifications, service alterations, or Windows OS setting changes during automated testing or report generation. All future system-modification logic must adhere to the 4-Layer Defense-in-Depth architecture defined in `docs/ARCHITECTURE.md`.
3. **Review Integrity:** Never claim ChatGPT has reviewed or approved a phase unless the Product Owner explicitly confirms it.
