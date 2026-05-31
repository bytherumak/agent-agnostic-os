# OS Instincts (Continuous Learning)

This file contains the hard-won lessons, obscure bugs, and absolute architectural constraints discovered during development. LLM agents MUST read this file before formulating a plan to avoid repeating past mistakes.

Whenever a task is completed, any new critical learnings must be appended here.

## Core Instincts
- **The Rename Trap:** Auto-generation migration tools (like Alembic, Prisma, Knex) often fail to detect column or table renames, issuing `drop` and `create` commands instead. This causes MASSIVE DATA LOSS in production. *Instinct:* Always manually inspect and edit migration files to use the explicit rename operation if a field is renamed.

## Project-Specific Instincts
*(Add new rules here as they are discovered during development. Each instinct should be a single, absolute rule with a brief context of why it exists.)*

- [YYYY-MM-DD] **[Topic]:** [Rule description and why it matters.]
