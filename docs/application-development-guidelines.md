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

## 6. Logging and error handling

- **Structured logging**: Include request id (or correlation id), log level, and context (e.g. route, user id when safe). Use a consistent format (JSON or key-value) so logs are searchable and parseable.
- **Never log sensitive data**: Do not log passwords, tokens, API keys, full PII (e.g. credit cards), or anything that could expose user or system secrets. Redact or omit.
- **User vs server messages**: User-facing error messages must be generic and safe (e.g. "Something went wrong. Please try again."). Do not expose stack traces, internal paths, or DB details to the client. Detailed errors belong in server logs and monitoring only.
- **Frontend**: Use error boundaries (e.g. React `ErrorBoundary`) to catch render errors and show fallback UI. For async failures (fetch, mutations), show retry or recovery guidance where appropriate.
- **Checklist**: (1) New API/logic logs with request id and level. (2) No secrets in logs or client responses. (3) User sees safe messages; details only in logs. (4) Critical UI paths have error boundaries or equivalent.

---

## 7. Environment and security

- **Environment variables**: Keep secrets, API keys, DB URL, etc. in env vars, not in code. Document required keys in `.env.example`; keep real values in `.env` (excluded from git).
- **Client**: Do not read server-only env in client code; keep sensitive values server-side.
- **CORS and headers**: Keep the project’s CORS and security header setup; change only when a new domain/path is needed and document it.

- **Security (hardening)**:
  - **Input validation**: Already required in §2 Backend/API; validate and escape all inputs; never trust client data.
  - **Auth and permissions**: Log auth failures, permission changes, and sensitive admin actions (no secrets in logs). Protect routes and check permissions per project approach.
  - **HTTPS and headers**: Enforce HTTPS in production; use HSTS and security headers (e.g. CSP, X-Frame-Options) per project or platform.
  - **Public APIs**: Consider rate limiting (return 429, `Retry-After`, and optional `X-RateLimit-*` headers) to prevent abuse.
  - **Checklist**: See [OWASP Top 10](https://owasp.org/www-project-top-ten/) and project security policy; when adding auth or new endpoints, ensure validation, logging, and headers are in place.

---

## 8. CI/CD and deploy

- **On PR or push**: Run lint, type-check, and tests (unit and, if applicable, E2E) in CI. Do not merge if CI fails.
- **On merge to main (or production branch)**: Use the project’s build and deploy pipeline (e.g. `.github/workflows/`, Vercel, or other). Document the workflow and any manual steps in README or §7.
- **Rollback**: Document or automate how to rollback a bad deploy. Keep §7 and README updated with run/test/deploy commands and paths so agents and humans use the same source.

---

## 9. Performance

- **Core Web Vitals**: Consider LCP (Largest Contentful Paint), INP (Interaction to Next Paint), and CLS (Cumulative Layout Shift). When adding features that affect them (e.g. large assets, layout shifts), measure and optimize if the project has performance targets or a budget.
- **Loading**: Use lazy loading for below-the-fold or heavy components and images where appropriate. Prefer code-splitting and dynamic import for large features.
- **Caching and assets**: Follow project patterns for caching (HTTP cache, client cache, or framework defaults). Optimize images and bundles (e.g. compression, modern formats) per project tooling.
- **Checklist**: (1) New routes or heavy UI do not regress LCP/INP/CLS without a reason. (2) Large assets are lazy-loaded or optimized. (3) If the project defines a performance budget, stay within it.

Why and frontend strategies: [performance-research.md](performance-research.md).

---

## 10. Documentation

- **README**: Must document how to run the app locally, run tests, lint, typecheck, build, and deploy. Keep README updated when commands or paths change.
- **§7 (Project-specific)**: In [AGENTS.md](../AGENTS.md), §7 is the single place for project paths, stack, and commands. When adding or changing routes, schema paths, or test commands, update §7 (and README if they are the source).
