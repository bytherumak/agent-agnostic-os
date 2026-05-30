# Operational Scratchpad (Impact Analysis & Plan)

The LLM MUST materialize its thought process here *before* writing any code.

## Current Plan: [Feature/Bug Name]

### Impact Analysis
- **Target:** [What is changing]
- **Affected Components:** [Which files/modules will be impacted based on ARCH_SPEC.md]
- **Potential Breakages:** [What could go wrong?]
- **Schema/Migration Impact:** [Will this require a database migration? Yes/No/N/A] *(Instruction for future AI: Specify the migration tool command used to verify. Example: "Requires Alembic migration", "Requires Prisma migration", "Requires Django migration", "N/A — no database in this project".)*

### Migration Risk Level
Assess the risk of this change using the following criteria:
- **LOW:** No DB/schema changes. Pure logic or UI. Rollback = simple `git revert`.
- **MEDIUM:** New DB columns/tables (additive only). Rollback = downgrade migration.
- **HIGH:** Destructive DB changes (column rename, type change, constraint). Requires data backup before execution.
- **CRITICAL:** Data migration, table drops, or schema changes on tables with >10k rows. Requires explicit human sign-off and a tested rollback script.

**Risk Level for this plan:** [LOW | MEDIUM | HIGH | CRITICAL]

### Rollback Strategy
#### 1. Code Rollback
- [How to revert the code changes. E.g., "Git revert commit Y" or "Cherry-pick commit Z"]

#### 2. Schema/Data Rollback
- [How to revert the database or data state. E.g., "Run migration downgrade command" or "N/A — no DB changes"] *(Instruction for future AI: Use the project's specific rollback command. Example: "alembic downgrade -1", "prisma migrate reset", "knex migrate:rollback", "N/A — no database".)*

#### 3. Config Rollback
- [How to revert configuration changes. E.g., "Remove X from `.env`" or "N/A — no config changes"]

### Execution Checklist
- [ ] **Step 1:** Read `[DEPENDENCY MANIFEST]` and `.ai/ARCH_SPEC.md` to verify dependencies. *(Instruction for future AI: Replace with the actual filename. Example: "pyproject.toml", "package.json", "go.mod".)*
- [ ] **Step 2:** Write/Update tests. Run `[TEST COMMAND]` AND verify coverage. Existing tests passing is NOT enough. You MUST write new tests targeting the specific happy-paths and edge-cases of your new logic. *(Instruction for future AI: Replace with the actual test command. Example: "pytest --cov", "npm test", "go test ./...", "cargo test".)*
- [ ] **Step 3:** Implement the core logic (NO lazy stubs or placeholders).
- [ ] **Step 4:** Run `[TEST COMMAND]` again with the new code and verify all tests pass.
- [ ] **Step 5:** Run `[LINT/FORMAT COMMAND]` and verify. *(Instruction for future AI: Replace with the actual lint/format command. Example: "pre-commit run --all-files", "npm run lint && npm run format", "golangci-lint run".)*
- [ ] **Step 6:** **CRITICAL (Empty Migration):** If this change involves schema migrations, manually read the generated migration file to verify it actually contains the intended operations. It MUST NOT be an empty migration. *(Instruction for future AI: Specify how to verify. Example: "Read versions/xxx.py and check for create_table/alter_column", "Read prisma/migrations/ for SQL statements". If the project has no database, delete this step.)*
- [ ] **Step 7:** **CRITICAL (Rename Trap):** Most auto-generation migration tools do NOT detect column/table renames and will issue `drop` + `create` commands instead, causing MASSIVE DATA LOSS. If you are renaming a field, you MUST manually edit the migration file to use the tool's rename operation. *(Instruction for future AI: Specify the rename operation. Example: "op.alter_column / op.rename_table for Alembic", "renameColumn in Knex". If the project has no database, delete this step.)*
- [ ] **Step 8:** **Data Loss Check:** Verify that no data loss will occur, or ensure explicit human authorization is documented.
- [ ] **Step 9:** **Indexing Check:** If the project uses a database, verify that appropriate indexes are added for any new query patterns. *(If the project has no database, delete this step.)*
- [ ] **Step 10:** Update `STATE.md` to COMPLETED and commit.
