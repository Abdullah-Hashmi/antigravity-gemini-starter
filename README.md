# Antigravity Gemini Starter

A universal, production-grade agentic baseline configuration for projects built with [Google Antigravity](https://antigravity.google) utilizing Gemini models. Designed for standalone use or clean multi-model coexistence alongside Anthropic's Claude (`CLAUDE.md`) and Cursor (`.cursorrules`).

---

## Architecture Overview

```
antigravity-gemini-starter/
├── .agents/
│   └── mcp_config.example.json   # Optional: Sample MCP configurations (stdio & remote HTTP)
├── GEMINI.md                     # Core universal agentic baseline for Gemini models
├── README.md                     # Architecture & customization documentation
├── .gitignore                    # Clean exclusions (OS, agent secrets, temp data)
└── [Optional Domain Folders]
    ├── apps/mobile/GEMINI.md     # (Optional) Nested scoped rules for mobile
    └── apps/api/GEMINI.md        # (Optional) Nested scoped rules for backend
```

### Why File Isolation Matters

1. **Deterministic Discovery:** Antigravity natively loads `GEMINI.md` and `AGENTS.md` by walking up the directory hierarchy from the current file to the repository root.
2. **Zero Cross-Tool Pollution:** Claude Code exclusively looks for `CLAUDE.md`, and Cursor looks for `.cursorrules`. By dedicating Gemini-specific operating constraints to `GEMINI.md`, teams can collaborate across different AI agents without rule collisions, redundant token consumption, or conflicting prompt styles.
3. **Budget Compliance:** Antigravity enforces a **24 KB** per-rule limit and an aggregate **20,000 token budget** for always-on rules. This starter baseline consumes ~4.5 KB (~1,100 tokens), preserving over 90% of your rules budget for downstream project context.

---

## What Works Out-of-the-Box vs. What Needs Config

| Capability | Activation State | Notes |
| :--- | :--- | :--- |
| **Subagent Orchestration** | **Native** (Zero Config) | Spawns `research`, `browser`, and `self` subagents without workspace configuration. |
| **Terminal Sandbox** | **Native** (Zero Config) | Sandboxed filesystem and process execution with explicit bypass triggers. |
| **Browser Inspection** | **Native** (Zero Config) | Native Chrome DevTools Protocol integration via the `browser` subagent. |
| **Domain-Specific Rules** | **Customizable** | Add your stack details to Section 5 of `GEMINI.md` or use nested `GEMINI.md` files. |
| **External Custom MCPs** | **Optional** | Only needed if connecting custom local database or proprietary API servers. |

---

## How to Use This Starter

### 1. Adopt into an Existing Project
Copy `GEMINI.md` into the root of your existing repository:
```bash
cp /path/to/antigravity-gemini-starter/GEMINI.md ./GEMINI.md
```

### 2. Configure Your Domain Stack
Open `GEMINI.md` and navigate to **Section 5: Domain-Specific Directives**. Uncomment the template and fill in your project's technology stack:
```markdown
### Active Stack: Mobile (React Native / Expo)
- Architecture: Feature-first module structure, offline-first SQLite synchronization.
- State Management: Zustand + TanStack Query.
- UI & Styling: NativeWind (Tailwind CSS).
- Verification Commands:
  - Lint: npm run lint
  - Typecheck: npx tsc --noEmit
  - Test: npm test
```

### 3. Multi-Package / Monorepo Scoping
For monorepos (e.g., Turborepo, Nx, or nested client/server repos), keep root `GEMINI.md` purely universal, and place nested `GEMINI.md` files in subdirectories:
```
my-monorepo/
├── GEMINI.md              <-- Universal baseline (planning, error handling, safety)
├── apps/
│   ├── web/
│   │   └── GEMINI.md      <-- Next.js, Tailwind, Playwright rules
│   └── mobile/
│       └── GEMINI.md      <-- Expo, React Native, Maestro rules
```
Antigravity automatically inherits root rules and applies directory-specific rules when the agent operates within that folder.

---

## Starter Rules Summary

- **Multi-Model Coexistence:** Strict scope isolation; cross-tool configuration files are respected and preserved.
- **Planning Threshold:** Formal implementation plans required for changes touching >2 files, dependencies, database schemas, or authentication boundaries.
- **Quality & Safety:** Strict typing, structured error boundaries, minimal external dependencies, and absolute prohibition of unprompted destructive shell commands.
- **Verification Protocol:** Mandatory linting, TypeScript type-checking (`tsc --noEmit`), and DOM/visual verification before task completion.

---

## License
MIT
