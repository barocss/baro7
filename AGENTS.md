# baro7 — AI Agent Entry Point

This repo is a **web application** template and skill source. **Any AI agent** (Cursor, Codex, Claude, etc.) should **read this document first** before adding or changing code, then apply the rules that match the work scope. All rules live in this repo; no Cursor-specific rules required.

---

## 0. On entry (For any AI agent)

- Before adding or changing code in this repo, read this document (AGENTS.md).
- **Token efficiency**: Read AGENTS.md and SKILL.md first; load other docs (e.g. agent-ui-structure-behavior-guidelines, application-development-guidelines) only for the scope you're working on.
- When touching **screens, tabs, layout, or scroll**: follow **§2** and **`docs/agent-ui-structure-behavior-guidelines.md`**.
- When adding a **full feature (API + data + UI)**: follow **§3–§6** and **`docs/application-development-guidelines.md`**.
- When using Cursor, the same content is available as skills; behavior matches this repo’s docs.
- When **splitting work by role**: follow the agent roles in [docs/agents-application-project.md](docs/agents-application-project.md) (Backlog, Spec, API, Data, UI, Implementation, Test, E2E, GitHub, Docs, Review). Invoke with “Act as **Role** …” and stay within that role’s scope.

---

## 1. Which doc applies

| Work scope | Apply | Description |
|------------|--------|-------------|
| **UI structure, layout, scroll** | [docs/agent-ui-structure-behavior-guidelines.md](docs/agent-ui-structure-behavior-guidelines.md) | Keep Layer 1–2 (or L1–L5); change only Layer 3 (or L6–L7). |
| **React/shadcn UI generation** | [docs/react-shadcn-ui-7layers.md](docs/react-shadcn-ui-7layers.md) | 7-layer mapping; generate content in L6/L7 only; keep L4/L5 intact. |
| **Full feature (API + data + UI)** | [docs/application-development-guidelines.md](docs/application-development-guidelines.md) | Feature flow, backend, data, testing. |

UI-only requests → apply the UI guidelines (and React/shadcn doc when generating React/shadcn UI). “Add this feature end-to-end” → apply application-development guidelines and, for the UI part, keep Layer 3 only.

---

## 2. UI structure and behavior (required)

- When creating or changing screens, tabs, layout, or scroll/fill areas, follow **`docs/agent-ui-structure-behavior-guidelines.md`**.
- **Principle**: Data and view may change; **scroll, fill, resize, and app shell/layout structure must not break**. “Add field”, “bind data”, etc. → change **Layer 3 (content/data)** only. Layer 1–2 only when the user explicitly asks for layout/structure changes.
- If the doc’s **§8 Project-specific** is filled, follow those paths and roles. If empty, infer Layer 1–2 from the codebase using the doc’s principles and layout rules.

---

## 3. Feature flow (full stack)

When adding a feature across **API + data + UI**, follow this order:

1. **Define** — What to store/expose; schema/API spec if needed.
2. **API** — Endpoints, request/response, validation, errors. Follow project route location.
3. **Data** — New tables/columns → update schema, add and run migrations.
4. **UI** — Layer 3 only. New screen: copy existing screen template or same layout pattern, then fill content.

For “add button” or “bind field” only, change **UI (Layer 3)** only.

---

## 4. Backend / API

- **Route location**: Follow the project’s API route location (e.g. `src/app/api/`, `src/routes/`, `pages/api/`). Document in §7.
- **Validation**: Validate body, query, params (e.g. zod); return 4xx and clear messages on failure.
- **Response**: Consistent shape on success; 4xx/5xx and distinguishable messages on error; no sensitive data in responses.
- **Auth**: Protect and check permissions using the project’s approach (session, JWT, etc.).

---

## 5. Data layer

- **Schema location**: Follow the project’s schema and migration paths (e.g. `src/lib/schema.ts`, `drizzle/`, `prisma/`).
- **On change**: Update schema, then add and run migrations. Keep table/column naming conventions.
- **With API**: API uses schema/ORM for CRUD; business logic in service layer if the project uses it.

---

## 6. Testing and verification

- Run tests that match what changed:
  - API only → that API’s unit/integration and E2E if any.
  - Data only → after migrations, related query/service tests.
  - UI only → frontend unit/E2E; confirm layout/scroll still work.
  - Full feature → API/data tests then UI/E2E.
- **Commands**: Use the project’s test/lint/typecheck commands (see §7 or README).

---

## 7. Project-specific (fill in)

Fill these with the project’s choices:

- **Package manager**: (e.g. pnpm / npm / yarn)
- **Stack**: (e.g. Next.js, React, Vue, Tailwind, shadcn/ui)
- **API route path**: (e.g. `src/app/api/`)
- **Schema / migration path**: (e.g. `src/lib/schema.ts`, `drizzle/`)
- **Test / verification commands**: (e.g. `pnpm test`, `pnpm test:e2e`, `pnpm lint`, `pnpm typecheck`)
- **CI / deploy**: (e.g. "Lint and test run in CI on PR; deploy on push to main" — or point to README)
- **Branch / commit rules**: (if any; or point to README)

README and §7 are the single source for run, test, and deploy commands; keep them updated. Project-specific rules override this file when they exist.
