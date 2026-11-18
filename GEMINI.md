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
- **Strict Typing:** Default to strict typing across all languages (e.g., TypeScript with `noImplicitAny: true`, Python with strict type hints). Do not introduce untyped escape hatches (`any`, `unknown` casts without guards) unless strictly unavoidable.
- **Robust Error Handling:** Async operations, network boundaries, and file I/O must have deterministic error handling (try/catch blocks or centralized framework error middleware/boundaries). Never swallow errors silently or log without rethrowing/handling.
- **Minimal Dependencies:** Prioritize native platform capabilities, standard libraries, and existing project dependencies before proposing new third-party packages.
- **Surgical Diffing:** Make targeted, minimal edits. Do not reformat untouched code, alter unrelated files, or strip existing comments and documentation.

---

## 4. Verification & Validation Protocol
- **Sandboxed Execution First:** Run commands in the default sandbox environment first. Do not request sandbox bypass unless network or elevated host resources are genuinely required.
- **Build & Static Analysis Checkpoint:** Run project-native linters, type checks (e.g., `tsc --noEmit`), and relevant unit tests after implementing functional changes. Fix all introduced warnings and errors before declaring a task complete.
- **UI & Browser Verification:** When UI or web verification is required, delegate to the `browser` subagent to validate responsive breakpoints, DOM accessibility, and visual layout before completing the task.
- **Root-Cause Remediation:** If a build, script, or runtime command fails, diagnose the root cause directly from the logs and error traces rather than re-running speculative command variations.

---

## 5. Domain-Specific Directives (Configured per Project)
<!-- 
Developers: Append project-specific stack guidelines below using this template.
For multi-package repositories / monorepos, prefer placing nested `GEMINI.md` files
inside each sub-package directory (e.g., `apps/mobile/GEMINI.md`) for localized scoping.

### Active Stack: [Mobile | Web | Backend | Game]
- Architecture: [e.g., Feature-based folder structure, Offline-first local SQLite cache]
- State Management: [e.g., Zustand, TanStack Query, Redux Toolkit]
- UI & Styling: [e.g., NativeWind, Tailwind CSS, Tamagui, Material UI]
- Verification Commands:
  - Lint: [e.g., npm run lint]
  - Typecheck: [e.g., npx tsc --noEmit]
  - Test: [e.g., npm test]
-->
