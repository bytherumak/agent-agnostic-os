# LLM Wiki Schema & Workflow (Agent-Agnostic OS)

This file defines the strict conventions and workflows for how any LLM agent (Gemini, Claude, Copilot, Cursor, etc.) should operate within this project.

## 0. PRIME DIRECTIVE
Leave the system in a better, verifiable state than you found it. This overrides all other rules.

## Core Philosophy (The 7 Pillars of Perfect Memory)
1. **Active Task Tracking (The RAM):** `.ai/STATE.md` and `.ai/PLAN.md` track the *current* micro-goal and thought process.
2. **Contextual Memory (The Brain):** `wiki/` captures the *Why*, and `.ai/ARCH_SPEC.md` dictates the *What* and *Where*.
3. **Executable Memory (The Sword):** `[TEST FRAMEWORK]` tests capture the *Behavior*. TDD is mandatory for bugfixes. *(Instruction for future AI: Specify the project's test runner and assertion style. Example: "pytest with pytest-cov", "Jest with React Testing Library", "go test", "cargo test".)*
4. **Historical Memory (The Gatekeeper):** Git + `[PRE-COMMIT / CI HOOKS]` physically block bad code. *(Instruction for future AI: Specify linter hooks, formatters, or CI gates. Example: "pre-commit with ruff + mypy", "husky with eslint + prettier", "GitHub Actions lint job".)*
5. **Release Memory (The Communicator):** `CHANGELOG.md` tracks *What changed* for humans.
6. **Structural Memory (The Shield):** `[TYPE SYSTEM]` is the absolute source of truth. *(Instruction for future AI: Specify the project's type enforcement mechanism. Example: "Python Type Hints (mypy --strict)", "TypeScript strict mode", "Go's static types", "Rust's borrow checker".)*
7. **Proactive Environment Sync (The Pulse):** The LLM autonomously polls upstream Git state to avoid stale context.

## 1. The Wake-Up & Handoff Protocol (Strict Sequence)
Whenever a new session starts or context is lost, the LLM MUST follow this loop BEFORE taking any action or writing code:
1. **The Pulse (Environment Scan):**
   - Run `git fetch` and `git status` to detect divergence.
   - Run `git log -n 3` to understand recent commits.
   - Scan `wiki/` for recent architectural updates.
   - **No Blind Overwrites:** Never overwrite a file based on in-context memory. You MUST read the file from disk immediately before modifying it.
   - **Merge Conflict Protocol:** If a Git command results in a Merge Conflict (e.g., `<<<<<<< HEAD` markers appear), you are STRICTLY FORBIDDEN from attempting to resolve it autonomously. You must instantly mark `STATE.md` as `[BLOCKED]`, log the conflicting files, and abort execution for human intervention.
2. **Wake-up (State Sync):** Read `.ai/STATE.md` and `wiki/log.md` to understand the current micro-goal. If Git divergence was found in step 1, autonomously update `.ai/PLAN.md` to accommodate the new reality.
3. **Plan (Impact Analysis & Constraints):**
   - Read `.ai/ARCH_SPEC.md` to map dependencies and boundaries.
   - Read `[DEPENDENCY MANIFEST]` to verify available packages/modules. *(Instruction for future AI: Specify the dependency file. Example: "pyproject.toml", "package.json", "go.mod", "Cargo.toml", "build.gradle".)*
   - Draft the impact analysis, affected components, and step-by-step logic in `.ai/PLAN.md`.
4. **Execute:** Modify Code -> Run Tests -> Update `.ai/STATE.md` to `[COMPLETED]` -> Commit.
*The state files MUST ALWAYS be updated before the code is touched.*

## 2. Technical Directives (The Law)
*   **The Dependency Directive:** You are **FORBIDDEN** from introducing new external libraries or packages unless explicitly authorized by the user. You must solve problems using the existing stack defined in `.ai/ARCH_SPEC.md` and `[DEPENDENCY MANIFEST]`.
*   **Immutable Infrastructure:** You are STRICTLY FORBIDDEN from modifying configuration files (`[CI/CD CONFIG FILES]`, `[DEPENDENCY MANIFEST]`, `[LINTER/FORMATTER CONFIG]`) to bypass failing tests or linters. Infrastructure changes require explicit human approval. *(Instruction for future AI: List the exact config filenames. Example: ".pre-commit-config.yaml, pyproject.toml, .github/workflows/", "package.json, .eslintrc, tsconfig.json".)*
*   **Schema Migrations:** NEVER edit migration files directly. Use the project's migration tool's auto-generation command exclusively. Always verify migration status before committing. *(Instruction for future AI: Specify your migration tool and its commands. Example: "alembic revision --autogenerate / alembic current", "prisma migrate dev", "knex migrate:make", "django makemigrations". If the project has no database or migrations, delete this bullet.)*
*   **Secrets & Auth Testing:** NEVER hardcode real tokens or credentials in tests or code. You MUST use mocking to mock dependencies, or use explicit dummy tokens (e.g., `TEST_TOKEN_123`) configured in a `[TEST ENV FILE]`. Whenever a new environment variable is required, you MUST automatically append a dummy version of it to `.env.example`. *(Instruction for future AI: Specify the mocking library and test env file. Example: "unittest.mock.patch / .env.test", "jest.mock / .env.test", "testify mocks / .env.test".)*
*   **Mock Mode First:** New external integrations (APIs, LDAP, payment gateways) must implement a `MOCK_MODE` toggle controlled by an environment variable before real credentials are used. The integration must be fully testable in mock mode.

## 3. The Anti-Laziness & Loop Prevention Directives (The Enforcer)
*   **No Placeholders:** You are strictly FORBIDDEN from using placeholders like `# ... existing code ...`, `// rest of the function`, `pass` (when replacing logic), or `// TODO` to skip work.
*   **Explicit Resolution:** Never suggest that the human user completes a task or fills in logic. If a task is within your capabilities, you MUST execute it fully. No "DIY" comments.
*   **Complete Execution:** When updating a function, class, or file, you must provide the **exact, fully functional code** needed to replace the old block seamlessly. No shortcuts.
*   **Loop Prevention:** If you fail a test, linter, or pre-commit hook 3 times in a row on the same task, YOU MUST STOP. Do not blindly retry. Update `.ai/STATE.md` status to `[BLOCKED]`, copy the exact error output into the file, and halt operations. Wait for human intervention or a more capable LLM to take over.

## 4. State Maintenance (Garbage Collection)
To prevent context window bloating, whenever a task is marked `[COMPLETED]`, you MUST:
1. Clear the `Current Micro-Goal`, `Blockers`, and `Attempted Solutions` sections in `STATE.md`, resetting them to their blank template values.
2. Clear the `Rejected Approaches` table rows (keep the header).
3. Move any valuable long-term lessons learned to `wiki/` (e.g., `wiki/rejected-approaches.md` or a relevant architecture page).
4. Reset `PLAN.md` to its blank template state.

## Formatting Conventions
*   Use GitHub Flavored Markdown.
*   Link to other wiki pages using relative paths or explicit file links.
