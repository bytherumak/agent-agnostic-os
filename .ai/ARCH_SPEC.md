# Technical Architecture Specification (The Map)

This file is the absolute source of truth for the project's Tech Stack and Domain Topography. LLMs must read this to understand *what* tools they are allowed to use and *where* files belong.

## 1. The Tech Stack (The What)
**Rule:** No new dependencies without human approval. You must solve problems using the existing stack.

*   **Language & Runtime:** `[LANGUAGE & RUNTIME]` *(Instruction for future AI: Specify the exact language, version, and runtime. Example: "Python 3.11+", "TypeScript 5.x with Node.js 20 LTS", "Go 1.22+", "Rust 1.78+ (stable)".)*
*   **Primary Framework:** `[PRIMARY FRAMEWORK]` *(Instruction for future AI: Specify the main application framework and version. Example: "FastAPI", "Next.js 14 (App Router)", "Gin", "Actix-web 4", "Spring Boot 3.2", "Express.js".)*
*   **Secondary Framework (if any):** `[SECONDARY FRAMEWORK]` *(Instruction for future AI: Specify any secondary framework running alongside the primary one. Example: "Aiogram 3.x for Telegram bot", "Discord.js for bot layer", "gRPC server". Delete this line if there is only one framework.)*
*   **Database:** `[DATABASE & ORM/DRIVER]` *(Instruction for future AI: Specify the database engine, driver, ORM, and migration tool. Example: "PostgreSQL via asyncpg + SQLAlchemy 2.0 (Async) + Alembic", "MongoDB via Mongoose", "PostgreSQL via Prisma", "SQLite via Diesel". Write "None — this project has no database" if applicable.)*
*   **Caching & State:** `[CACHING LAYER]` *(Instruction for future AI: Specify caching/state management tools. Example: "Redis", "Memcached", "In-memory LRU cache", "None". Delete this line if no caching is used.)*
*   **Syntax Strictness (No Legacy Code):**
    - `[SYNTAX RULE 1]` *(Instruction for future AI: Specify a mandatory modern syntax rule and explicitly ban its legacy equivalent. Example: "Pydantic V2 only — @field_validator, model_dump(). V1 syntax (@validator, dict()) is STRICTLY BANNED", "Use ES modules (import/export). CommonJS require() is STRICTLY BANNED", "Use Go generics where applicable. Interface{} for known types is BANNED".)*
    - `[SYNTAX RULE 2]` *(Instruction for future AI: Add more syntax rules as needed. Example: "SQLAlchemy 2.0 Mapped[type] + mapped_column() only. Legacy Column() and session.query() are BANNED. Use select()." Delete if not applicable.)*
*   **Strict Rules:**
    - `[I/O CONCURRENCY RULE]` *(Instruction for future AI: Define the concurrency model. Example: "ALL database calls and I/O MUST be asynchronous. Never use blocking libraries like requests or synchronous SQLAlchemy. Use aiohttp or httpx.", "All API calls must use goroutines, never block the main thread.", "Use async/await throughout. Synchronous fs calls are BANNED in request handlers." Write "N/A — synchronous project" if blocking I/O is acceptable.)*
    - **Data Privacy & Logging:** NEVER log sensitive information in plain text. Passwords, JWTs, API keys, auth tokens, and user PII must be masked or completely omitted from all logger outputs.
    - **Timestamps:** All dates and times across the database, backend, and API responses MUST be stored and transmitted in UTC using the ISO-8601 format.

## 2. The Directory Map (The Where)
LLMs must strictly follow this map when creating or modifying files:

*(Instruction for future AI: Replace this entire section with the project's actual directory structure, 3 levels deep. Use the format below as a starting template and adapt it to your architecture.)*

*   `[SRC_DIR]/core/` - Global configurations, settings, and shared utilities.
*   `[SRC_DIR]/api/` - API routes, controllers, and endpoint definitions. *(Delete if the project has no HTTP API.)*
*   `[SRC_DIR]/domain/` - Core business logic, services, and use cases (independent of transport/UI layers).
*   `[SRC_DIR]/infrastructure/` - External integrations (e.g., LDAP, API Gateways, email providers).
*   `[SRC_DIR]/db/` - Database models, schemas, and migration files. *(Delete if the project has no database.)*
*   `[FRONTEND_DIR]/` - Frontend/UI source code. *(Instruction for future AI: Specify the frontend technology. Example: "React SPA", "Svelte app", "Telegram Mini App (TMA)", "CLI interface". Delete this line if no frontend exists.)*
*   `[TEST_DIR]/` - Test suites (mirroring the `[SRC_DIR]/` structure).

## 3. Integration Contracts (The Bridge)
*(Instruction for future AI: Replace the integration descriptions below with the project's actual integration points. Keep the universal rules — Idempotency, Error Contract, and the shared-state guidance — and adapt the rest.)*

*   **Frontend to Backend:** `[FRONTEND_TYPE]` communicates with `[BACKEND_FRAMEWORK]` via `[PROTOCOL]`. *(Example: "The React SPA communicates with the Express API via REST", "The TMA communicates with FastAPI via REST", "The mobile app uses gRPC". Delete this line if there is no frontend.)*
*   **Authentication:** `[AUTH METHOD]` *(Instruction for future AI: Describe the authentication mechanism. Example: "JWT Bearer tokens validated by middleware", "TMA initData validated via Bot Token", "OAuth 2.0 PKCE flow with refresh tokens", "API key in X-API-Key header". Delete this line if no auth is needed.)*
*   **Async Boundaries:** `[CONCURRENCY BOUNDARY RULE]` *(Instruction for future AI: Describe how concurrent subsystems share state. Example: "The bot and API server run concurrently; shared state must go through Redis or the database.", "The worker processes communicate via a message queue (RabbitMQ).", "N/A — single-process application". Delete this line if the project has a single execution context.)*
*   **Idempotency:** All endpoints and handlers MUST gracefully handle duplicate requests/double-clicks without corrupting state or crashing.
*   **API Error Contract:** `[ERROR RESPONSE SCHEMA]` *(Instruction for future AI: Define the standard error response format. Example: '{"error_code": "STRING_CODE", "message": "Human readable description", "details": {}}', '{"error": {"code": 400, "message": "...", "errors": []}}'. Adapt to match your API's conventions.)*

## 4. Domain Glossary (The Business Logic)
*(Company/project-specific business terms to prevent generic LLM hallucinations. Replace the placeholders below with the project's actual domain language.)*

*   `[DOMAIN ENTITY 1]`: `[Definition]` *(Example: "Employee — A user synced from the corporate LDAP directory, mapped to a Telegram account via phone number.")*
*   `[DOMAIN ENTITY 2]`: `[Definition]` *(Example: "Workspace — An isolated tenant environment containing its own users, settings, and data.")*
*   `[DOMAIN PROCESS 1]`: `[Definition]` *(Example: "Onboarding — The 3-step flow where a new employee links their Telegram to their corporate profile.")*
*   **Soft Delete:** NEVER drop or delete records from the database. Always use `is_active=False` (or equivalent soft delete pattern) for users, items, and relationships. *(Instruction for future AI: If the project has no database, delete this entry. If the project uses a different soft-delete mechanism (e.g., `deleted_at` timestamp), specify it here.)*

## 5. Critical Architectural Invariants
These rules are absolute and may NEVER be violated under any circumstances:

*(Instruction for future AI: Replace the examples below with the project's actual invariants — the rules that, if violated, WILL break production. Be specific and exhaustive.)*

*   `[CRITICAL INVARIANT 1]` *(Example: "All database calls must use async engines (AsyncSession, async_engine) exclusively. Synchronous blocking I/O inside FastAPI or Aiogram handlers is strictly forbidden.")*
*   `[CRITICAL INVARIANT 2]` *(Example: "No handler, route, or service function may call time.sleep(), synchronous requests.get(), or any other blocking call. Use asyncio.sleep(), httpx.AsyncClient, or equivalent async primitives.")*
*   `[CRITICAL INVARIANT 3]` *(Example: "The event loop must never be blocked. If a CPU-bound task is unavoidable, it must be offloaded to a thread pool via asyncio.to_thread() or a dedicated task queue.")*
*   `[CRITICAL INVARIANT 4]` *(Example: "All API responses must include cache-control headers. No endpoint may return unbounded result sets without pagination.")*
