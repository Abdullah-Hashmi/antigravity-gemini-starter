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
3. **Budget Compliance:** Antigravity enforces a **24 KB** per-rule limit and an aggregate **20,000 token budget** for always-on rules. This starter baseline consumes ~6 KB on disk but only ~4.8 KB (~1,200 tokens) at runtime (the Section 5 comment template costs zero tokens), preserving over 90% of your rules budget for downstream project context.

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
Open `GEMINI.md` and navigate to **Section 5: Domain-Specific Directives**. Uncomment the template and fill in your project's technology stack. Here are examples for different domains:

**Mobile:**
```markdown
### Active Stack: Mobile (React Native / Expo)
- Architecture: Feature-first module structure, offline-first SQLite synchronization.
- Language & Runtime: TypeScript 5.x / Node 22.
- Frameworks: Expo 52, React Native 0.76.
- State Management: Zustand + TanStack Query.
- UI & Styling: NativeWind (Tailwind CSS).
- Verification Commands:
  - Lint: npm run lint
  - Typecheck: npx tsc --noEmit
  - Test: npm test
  - Build: npx expo export
- Dev Server: (not applicable — uses Expo Go / device preview)
- Breakpoints: (not applicable)
```

**Web:**
```markdown
### Active Stack: Web (Next.js)
- Architecture: App Router with server components, feature-based module structure.
- Language & Runtime: TypeScript 5.x / Node 22.
- Frameworks: Next.js 15, React 19.
- State Management: TanStack Query + Zustand.
- UI & Styling: Tailwind CSS v4.
- Verification Commands:
  - Lint: npm run lint
  - Typecheck: npx tsc --noEmit
  - Test: npx vitest run
  - Build: npm run build
- Dev Server: npm run dev -- --port 3000
- Breakpoints: 375, 768, 1024, 1440
- Environment:
  - Required Vars: DATABASE_URL, NEXTAUTH_SECRET
  - Config File: .env.local
```

**Game:**
```markdown
### Active Stack: Game (Unity 2022 LTS)
- Architecture: Component-based ECS, scene-per-level, ScriptableObject data layer.
- Language & Runtime: C# 11 / .NET 6.
- Frameworks: Unity 2022 LTS, Universal Render Pipeline.
- State Management: ScriptableObject event channels + singleton service locators.
- UI & Styling: UI Toolkit (USS) + in-engine theme.
- Verification Commands:
  - Lint: dotnet format
  - Typecheck: (handled by Unity compiler)
  - Test: Unity Test Runner (Edit Mode + Play Mode)
  - Build: Unity -batchmode -buildTarget StandaloneLinux64 -executeMethod BuildScript.Build
- Dev Server: (not applicable)
- Breakpoints: (not applicable)
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
- **Verification Protocol:** Mandatory project-native linting and type-checking, with optional dev-server lifecycle and browser verification for UI-touching changes.

---

## License
MIT
