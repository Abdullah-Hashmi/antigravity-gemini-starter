# GEMINI.md - Universal Agentic Baseline

This document defines system constraints, operating protocols, and execution standards for Gemini agentic workflows within this project.

---

## 1. Multi-Model Coexistence Rules
- **Scope Isolation:** These instructions apply strictly to Gemini models operating within this workspace.
- **Cross-Tool Respect:** Do not modify, overwrite, or deprecate configurations intended for other agents (e.g., `CLAUDE.md`, `.cursorrules`).
- **Conflict Resolution:** If generic files like `AGENTS.md` exist, prioritize directives here for Gemini reasoning sessions while maintaining global compatibility.

---

## 2. Agentic Workflow & Planning Standard
- **Plan Before Execute:** Emit a formal Implementation Plan before making changes if the task involves:
  1. Modifying more than 2 files.
  2. Adding or updating dependencies.
  3. Modifying database schemas, migrations, or persistent models.
  4. Modifying authentication, authorization, or security boundaries.
  *The plan must state: scope of work, affected files, potential side effects, and concrete validation steps.*
- **Subagent Delegation:**
  - Offload deep codebase search, extensive external documentation lookup, or multi-page scraping to the `research` subagent.
  - Offload live web inspection and DOM validation to the `browser` subagent.
  - Synthesize and return only condensed, high-signal summaries to the primary context to prevent context window degradation.
  - Rely on reactive system notifications for subagent completion—never poll or run artificial wait loops.
- **Idempotence & State Preservation:** Never execute destructive terminal commands (`rm -rf`, database drops, table truncations, cache purges) without explicit user authorization. Always prefer non-destructive alternatives (moving to a temporary location, targeted deletions).

---

## 3. Code Quality & Architecture Defaults
- **Reasoning Before Action:** Always explain the architectural reasoning (*why*) before presenting implementation details (*how*).
- **Strict Typing:** Default to strict typing in every language the project uses (e.g., TypeScript `strict: true`, Python strict type hints + `pyright`, Rust default borrow checker, C# nullable reference types, Kotlin null-safety). Do not introduce untyped escape hatches (`any`, `unknown`, unchecked casts, `as!`, `!!`) unless strictly unavoidable and annotated with a justification comment.
- **Robust Error Handling:** Async operations, network boundaries, and file I/O must have deterministic error handling (try/catch blocks or centralized framework error middleware/boundaries). Never swallow errors silently or log without rethrowing/handling.
- **Minimal Dependencies:** Prioritize native platform capabilities, standard libraries, and existing project dependencies before proposing new third-party packages.
- **Surgical Diffing:** Make targeted, minimal edits. Do not reformat untouched code, alter unrelated files, or strip existing comments and documentation.

---

## 4. Verification & Validation Protocol
- **Sandboxed Execution First:** Run commands in the default sandbox environment first. Do not request sandbox bypass unless network or elevated host resources are genuinely required.
- **Build & Static Analysis Checkpoint:** Run the project's configured verification commands (see §5 `Verification Commands`) after implementing functional changes. When no project-specific commands are configured, run the language-native defaults (linter, type checker, test runner). Fix all introduced warnings and errors before declaring a task complete.
- **Dev-Server Lifecycle (Web / UI projects):** When the project declares a web or full-stack stack in §5, follow this sequence for any UI-touching change:
  1. Start the project's dev server (see §5 `Dev Server`) in the background.
  2. Delegate `localhost:<port>` to the `browser` subagent for visual and DOM verification.
  3. After verification completes, tear down the dev server.
  Do not probe the dev server with `curl`, `wget`, or manual HTTP — always delegate to the `browser` subagent.
- **UI & Browser Verification:** When UI or web verification is required, delegate to the `browser` subagent to validate:
  - Visual layout and responsive behavior at declared breakpoints (see §5 `Breakpoints`, or default to `[375, 768, 1280]` px widths).
  - DOM accessibility: semantic HTML, focus order, ARIA attributes, color contrast.
  - Console errors: no unhandled exceptions or failed network requests.
- **Root-Cause Remediation:** If a build, script, or runtime command fails, diagnose the root cause directly from the logs and error traces rather than re-running speculative command variations.

---

## 5. Domain-Specific Directives (Configured per Project)
<!-- 
Developers: Uncomment and fill ONE OR MORE stack blocks below.
Verification Commands, Dev Server, and Breakpoints are used by Section 4
to drive automated quality gates — the agent reads these values at runtime.

For monorepos, prefer nested GEMINI.md files inside each sub-package
(e.g., apps/web/GEMINI.md, apps/mobile/GEMINI.md) for localized scoping.

### Active Stack: [Web | Mobile | Backend | Game | CLI | Library]
- Architecture: [e.g., Feature-based modules, Clean Architecture, ECS]
- Language & Runtime: [e.g., TypeScript 5.x / Node 22, Python 3.12, C# 11 / .NET 6, Rust 1.78, GDScript 4.x]
- Frameworks: [e.g., Next.js 15, Expo 52, Flask 3, Unity 2022 LTS, Bevy 0.14]
- State Management: [e.g., Zustand, Redux Toolkit, Riverpod, Vuex, ScriptableObject channels]
- UI & Styling: [e.g., Tailwind CSS, NativeWind, Material UI, UI Toolkit (USS), in-engine theme]
- Verification Commands:
  - Lint: [e.g., npm run lint | ruff check . | dotnet format]
  - Typecheck: [e.g., npx tsc --noEmit | pyright | (handled by compiler)]
  - Test: [e.g., npm test | pytest | dotnet test | Unity Test Runner]
  - Build: [e.g., npm run build | cargo build | dotnet publish]
- Dev Server: [e.g., npm run dev -- --port 3000 | (not applicable for mobile/game)]
- Breakpoints: [e.g., 375, 768, 1024, 1440 | (not applicable)]
- Environment:
  - Required Vars: [e.g., DATABASE_URL, API_KEY | (none)]
  - Config File: [e.g., .env.local | (none)]
-->
