# Application Development (Full Stack)

**Any AI agent** that adds or changes features across API, data, and UI in this repo should follow this document. For UI layout, scroll, and shell only, also follow [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) and keep Layer 1–2 intact.

---

## 1. Feature flow

When adding one feature, keep this **order**:

1. **Define**: Decide what to store/expose. Document schema/API spec in comments or docs if needed.
2. **API**: Add endpoints, request/response, validation, and error handling. Follow the project’s route location and patterns.
3. **Data**: If new tables/columns or entities are needed, update the schema, add migrations, then use them in the API.
4. **UI**: Add or change only **Layer 3 (content/data)** — screens, forms, tables. Do not change app shell, tabs, or scroll/fill wrappers (Layer 1–2) for “add feature”. For a new screen, copy the existing screen template or the same layout pattern, then fill content only.

If the request is “add one button” or “bind field”, change **UI (Layer 3)** only; do not touch API, data, or shell.

---

## 2. Backend / API

- **Location**: Follow where the project defines API routes (e.g. `src/app/api/`, `src/routes/`, `pages/api/`, `app/api/`).
- **One endpoint**: One route file/handler per path and method. Use middleware or wrappers for shared validation, auth, and error handling.
- **Request**: Validate body, query, and params with a schema (zod, yup, etc.); return 400 or similar with a clear message on failure.
- **Response**: Return a consistent shape on success (e.g. `{ data }` or `{ items }`). Follow pagination/sort rules for lists when applicable.
- **Errors**: Return 4xx/5xx with a code and message the client can use; do not expose sensitive information.
- **Auth**: If the project uses session, JWT, API keys, etc., protect routes and check permissions accordingly.

---

## 3. Data layer

- **Schema location**: Follow the project’s schema definition location (e.g. `src/lib/schema.ts`, `drizzle/`, `prisma/schema.prisma`, `db/schema.ts`).
- **Add/change**: Reflect new tables, columns, and indexes in the schema, then **generate and apply migrations**. If the project uses raw SQL only, follow its migration file rules.
- **Naming**: Keep table and column naming conventions (snake_case, camelCase, etc.). Keep relationships (foreign keys) clear with existing entities.
- **With API**: Prefer API handlers using schema/ORM for CRUD and keeping business logic in a service/util layer. If the project already has a layer split, follow it.

---

## 4. Testing and verification

- **Run tests that match what changed**:
  - **API only** → that API’s unit/integration tests and any related E2E.
  - **Schema/data only** → after applying migrations, related query/service tests.
  - **UI only (Layer 3)** → frontend unit/snapshot/E2E; confirm layout/scroll are still correct.
  - **Full feature (API + data + UI)** → in order: schema/API tests then UI/E2E.
- **Commands**: Use the project’s test/lint/typecheck commands. Prefer what is documented in README or [AGENTS.md](../AGENTS.md).
- **On failure**: Fix the failing test or, if behavior changed intentionally, update spec/test expectations first, then align code.

---

## 5. UI (summary)

- Follow [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) for UI structure, scroll, and shell.
- When adding a feature, change **Layer 3 only** on the UI side; keep Layer 1–2 (shell, tabs, scroll/fill wrappers) unchanged.

---

## 6. Environment and security

- **Environment variables**: Keep secrets, API keys, DB URL, etc. in env vars, not in code. Document required keys in `.env.example`; keep real values in `.env` (excluded from git).
- **Client**: Do not read server-only env in client code; keep sensitive values server-side.
- **CORS and headers**: Keep the project’s CORS and security header setup; change only when a new domain/path is needed and document it.
