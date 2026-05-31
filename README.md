<div align="center">

# 🧠 Agent-Agnostic OS

### The Universal Operating System for LLM-Driven Autonomous Development

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Agent Agnostic](https://img.shields.io/badge/Agent-Agnostic-purple.svg)](#)

*Stop losing context. Stop repeating mistakes. Start shipping.*

---

**GPT-4o · Claude · Gemini · Copilot · Cursor · Codex · Any LLM**

</div>

---

## The Problem

Every AI coding agent suffers from the same fatal flaw: **amnesia**.

Each new session starts from zero. The agent doesn't know what was tried yesterday, what broke last week, or why a particular architectural decision was made three sprints ago. The result? Circular debugging, violated boundaries, phantom dependencies, and an ever-growing pile of "the AI broke it again" commits.

**Agent-Agnostic OS** is a structured boilerplate that solves this by giving *any* LLM agent a persistent memory layer, strict operational protocols, and hard architectural boundaries — all through plain Markdown files that live in your repo.

It is not a library. It is not a framework. It is an **operating system for AI agents**, and it works with every IDE and every model on the market.

---

## ✨ Key Principles

| Principle | What It Means |
|---|---|
| **🧠 Persistent Memory** | State, plans, and architectural knowledge survive across sessions, models, and even IDEs. No more "starting from scratch." |
| **🔒 Strict Boundaries** | The agent cannot introduce new dependencies, modify infrastructure configs, or bypass failing tests without human approval. |
| **🔄 Agent Handoff** | Mid-task, you can switch from Claude to Gemini to GPT — the incoming agent reads `STATE.md` and picks up exactly where the last one left off. |
| **🛡️ Anti-Laziness Enforcement** | No placeholders. No `// TODO`. No "the user should fill this in." The agent must execute fully or declare itself blocked. |
| **📜 Technology Agnostic** | Works with Python, TypeScript, Go, Rust, Java, or any stack. You fill in the specifics; the OS provides the structure. |

---

## 🔌 How It Works: The Boot Chain

When an AI agent opens your project, a deterministic three-stage boot sequence fires — automatically, silently, and without any user intervention.

```
┌─────────────────────────────────────────────────────────────────┐
│                        BOOT SEQUENCE                            │
│                                                                 │
│  ┌──────────────┐    ┌───────────────────┐    ┌──────────────┐  │
│  │              │    │                   │    │              │  │
│  │ .cursorrules │───▶│ AI_INSTRUCTIONS   │───▶│  STATE.md    │  │
│  │ (Bootloader) │    │ (Kernel)          │    │  PLAN.md     │  │
│  │              │    │                   │    │  ARCH_SPEC   │  │
│  └──────────────┘    └───────────────────┘    └──────────────┘  │
│                                                                 │
│  Stage 1: BIOS       Stage 2: Kernel Load    Stage 3: Init     │
└─────────────────────────────────────────────────────────────────┘
```

### Stage 1 — The Bootloader (`.cursorrules`)

The `.cursorrules` file is a single, carefully crafted instruction that most modern AI-aware IDEs (Cursor, Windsurf, Copilot, etc.) automatically inject into the agent's system prompt on startup. It contains one directive:

> *"Before doing anything, silently read `.ai/AI_INSTRUCTIONS.md` and execute the Wake-Up Protocol."*

The agent never announces this. It just obeys.

> **💡 Portability Note:** If your IDE uses a different convention (e.g., `.github/copilot-instructions.md`, `AGENTS.md`, `.clinerules`), simply copy the bootloader content into that file. The OS adapts to your toolchain — not the other way around.

### Stage 2 — The Kernel (`.ai/AI_INSTRUCTIONS.md`)

This is the brain of the OS. It defines:

- **The Prime Directive** — *"Leave the system in a better, verifiable state than you found it."*
- **The 7 Pillars of Perfect Memory** — A philosophy covering active task tracking, contextual memory, executable memory (tests), historical memory (Git), release memory (changelogs), structural memory (types), and environment sync.
- **The Wake-Up & Handoff Protocol** — A strict 5-step sequence the agent must follow before writing a single line of code:
  1. **The Pulse** — `git fetch`, `git status`, `git log -n 3`. Detect divergence. Never overwrite blindly.
  2. **Wake-up** — Read `STATE.md` to synchronize with the current micro-goal.
  3. **Skill Loading** — Scan `skills/` for relevant reusable workflows and micro-agents.
  4. **Plan** — Read `ARCH_SPEC.md` and `INSTINCTS.md`, draft an impact analysis in `PLAN.md`.
  5. **Execute** — Code → Test → Update State → Commit.
- **Technical Directives** — Hard laws governing dependency management, infrastructure immutability, schema migrations, secrets handling, and mock-first integrations.
- **Anti-Laziness & Loop Prevention** — No placeholders, no "DIY" comments, no infinite retry loops. If the agent fails 3 times, it must stop and declare itself `[BLOCKED]`.

### Stage 3 — State Initialization

The agent reads the three operational files (see below) to understand *where we are*, *what's the plan*, and *what's the architecture*.

---

## 📂 The Core Structure

The `.ai/` directory contains everything the agent needs to operate autonomously:

```
.ai/
├── AI_INSTRUCTIONS.md   # 🧠 The Kernel — Protocols, laws, and workflow rules
├── STATE.md             # 💾 The RAM — Current task, status, and blockers
├── PLAN.md              # 📋 The Scratchpad — Impact analysis and execution checklist
├── ARCH_SPEC.md         # 🗺️ The Map — Tech stack, directory layout, and invariants
├── INSTINCTS.md         # 🛡️ The Immune System — Hard-won lessons and mistakes to avoid
└── skills/              # 🛠️ The Toolbelt — Reusable agent workflows and scripts
    └── SKILL_TEMPLATE.md
```

### `AI_INSTRUCTIONS.md` — The Kernel

The operating system itself. Contains the Prime Directive, the Wake-Up Protocol, Technical Directives (The Law), Anti-Laziness Directives (The Enforcer), and State Maintenance rules (Garbage Collection). **You should rarely need to edit this file** — it is stack-agnostic by design.

### `STATE.md` — The RAM (Working Memory)

The agent's short-term memory. Tracks:

- **Session Metadata** — Which model last touched the project, which Git branch is active.
- **Current Micro-Goal** — The single, atomic task in progress.
- **Status** — `IN_PROGRESS`, `COMPLETED`, or `BLOCKED`.
- **Blockers & Attempted Solutions** — What went wrong, what was already tried (so the next agent doesn't repeat it).
- **Rejected Approaches** — A persistent log of dead ends with exact failure reasons.
- **Handoff Notes** — Explicit instructions for the next agent if the session drops mid-task.

This file is the **anti-amnesia mechanism**. When a new session starts — even with a completely different model — the agent reads `STATE.md` and knows exactly where to pick up.

### `PLAN.md` — The Impact Analysis Scratchpad

Before writing any code, the agent must materialize its thinking here:

- **Impact Analysis** — What's changing, which components are affected, what could break.
- **Migration Risk Level** — LOW / MEDIUM / HIGH / CRITICAL, with clear criteria for each.
- **Rollback Strategy** — Code rollback, schema rollback, and config rollback plans.
- **Execution Checklist** — A step-by-step checklist: read dependencies → write tests → implement → run tests → lint → verify migrations → update state.

This ensures the agent *thinks before it acts* — no cowboy coding.

### `ARCH_SPEC.md` — The Tech Stack Map

The architectural constitution of your project. Defines:

- **The Tech Stack** — Language, framework, database, ORM, caching layer, with strict syntax rules and banned patterns.
- **The Directory Map** — Where every type of file belongs. The agent cannot create files outside this structure.
- **Integration Contracts** — How frontend talks to backend, authentication flows, concurrency boundaries, error response schemas.
- **Domain Glossary** — Business-specific terminology to prevent LLM hallucinations (e.g., what "Employee" or "Workspace" means in *your* domain).
- **Critical Invariants** — Absolute rules that may never be violated (e.g., "all I/O must be async", "no unbounded queries without pagination").

**This is the file you customize.** It ships with `[GUIDED PLACEHOLDERS]` that tell you exactly what to fill in, with examples for every major stack.

### `INSTINCTS.md` — The Immune System

The project's continuous learning file. Auto-generation tools and LLMs often repeat the same specific mistakes (like dropping columns instead of renaming them in database migrations). Whenever a hard-won lesson is learned or a critical bug is fixed, the agent appends it here. Future agents must read this before planning to avoid repeating history.

### `skills/` — The Toolbelt

A directory for storing executable instructions, bash scripts, and API interaction templates. If there's a complex multi-step process you do often (like "Deploy to Staging" or "Add a new Database Model"), you define it as a skill using the `SKILL_TEMPLATE.md`. Agents can then execute these skills flawlessly without needing you to explain the steps every time.

---

## 🚀 Quick Start

### 1. Clone the Template

```bash
git clone https://github.com/bytherumak/agent-agnostic-os.git my-project
cd my-project
rm -rf .git
git init
```

### 2. Configure `ARCH_SPEC.md`

Open `.ai/ARCH_SPEC.md` and replace every `[GUIDED PLACEHOLDER]` with your project's specifics. Each placeholder includes an instruction and real-world examples:

```markdown
# Before (placeholder):
*   **Language & Runtime:** `[LANGUAGE & RUNTIME]`
    *(Instruction for future AI: Specify the exact language, version, and runtime.
    Example: "Python 3.11+", "TypeScript 5.x with Node.js 20 LTS")*

# After (your project):
*   **Language & Runtime:** Python 3.12+ (CPython)
```

Repeat for every placeholder across all four sections: Tech Stack, Directory Map, Integration Contracts, and Domain Glossary.

> **💡 Tip:** Delete any sections that don't apply (e.g., remove the Database section if your project has no database, remove the Frontend line if it's a CLI tool).

### 3. Adapt the Bootloader

If your IDE doesn't use `.cursorrules`, copy its content to the appropriate file:

| IDE / Agent | Config File |
|---|---|
| Cursor | `.cursorrules` ✅ (already included) |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Windsurf | `.windsurfrules` |
| Cline | `.clinerules` |
| Codex | `AGENTS.md` |
| Generic | Paste into the agent's system prompt |

### 4. Start Building

Open your IDE, start a chat with your AI agent, and give it a task. The boot chain fires automatically. The agent will:

1. Silently read the bootloader → kernel → state files.
2. Run `git fetch` and `git status` to sync with reality.
3. Load relevant skills and draft an impact analysis in `PLAN.md`.
4. Execute the task following the strict checklist.
5. Update `STATE.md` and append any new learnings to `INSTINCTS.md`.

**That's it.** Your AI agent now has persistent memory, architectural awareness, and self-enforcing guardrails.

---

## 🔄 Multi-Agent Handoff

One of the most powerful features of the OS is seamless handoff between different AI models:

```
Session 1 (Claude)           Session 2 (GPT-4o)          Session 3 (Gemini)
┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
│ Reads STATE.md   │         │ Reads STATE.md   │         │ Reads STATE.md   │
│ Status: -        │         │ Status: BLOCKED  │         │ Status: IN_PROG  │
│                  │         │                  │         │                  │
│ Implements auth  │         │ Finds blocker    │         │ Fixes edge case  │
│ Tests fail on    │         │ Tries new angle  │         │ All tests pass   │
│ edge case        │         │ Partial progress │         │                  │
│                  │         │                  │         │                  │
│ Updates STATE:   │         │ Updates STATE:   │         │ Updates STATE:   │
│ BLOCKED + error  │────────▶│ IN_PROGRESS      │────────▶│ COMPLETED        │
│ + attempted fix  │         │ + new approach   │         │ + clears state   │
└──────────────────┘         └──────────────────┘         └──────────────────┘
```

Each agent inherits the full context of its predecessors — including what was tried and why it failed. No repeated mistakes. No lost progress.

---

## 🏗️ Project Structure

```
your-project/
├── .ai/
│   ├── AI_INSTRUCTIONS.md    # The Kernel (rarely edited)
│   ├── STATE.md              # Working Memory (auto-managed by agents)
│   ├── PLAN.md               # Impact Analysis (auto-managed by agents)
│   ├── ARCH_SPEC.md          # Tech Stack Map (YOU configure this)
│   ├── INSTINCTS.md          # Immune System (learned mistakes)
│   └── skills/               # Reusable agent workflows
├── .cursorrules              # The Bootloader
├── wiki/                     # Long-term knowledge base (created as needed)
│   ├── log.md                # Architectural decision log
│   └── rejected-approaches.md
├── CHANGELOG.md              # Release history (created as needed)
└── ... your source code ...
```

---

## 🤝 Contributing

Contributions are welcome! Whether it's improving the protocol, adding bootloader configs for new IDEs, or sharing your customized `ARCH_SPEC.md` templates for specific stacks — open a PR.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-improvement`)
3. Commit your changes (`git commit -m 'Add amazing improvement'`)
4. Push to the branch (`git push origin feature/amazing-improvement`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built for humans who work with AI. Designed for AI that works with humans.**

*Stop the amnesia. Ship with confidence.*

⭐ Star this repo if it saved your sanity.

</div>

