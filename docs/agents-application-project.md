# baro7 — Agent Roles for Application Projects

This document defines **agent roles** for **this project (web application)**. Use them on any AI platform (Cursor, Codex, Claude, etc.) by role name; everything stays in the repo.

---

## 1. Why project-specific agents

- **barocss-editor**: Editor platform → Spec, Implementation (model, extension, view), Test, E2E, GitHub, Docs — roles for the **editor domain**.
- **baro-corp**: Single-person company service → plan, spec, code, check, release, doc — agents by **report/artifact area**.
- **baro7**: Web app build and run → layers are **API / data / UI**, and “one feature” often touches all three; roles are aligned to **app development flow**.

---

## 2. Role list (Focus · Input · Output)

| Role | Scope | Input | Output |
|------|--------|--------|--------|
| **Backlog** | Issue and task management | User request (create/prioritize/triage) | Issue list, labels, report |
| **Spec** | Feature definition, API, screen spec | Request, existing specs | Spec doc, implementation checklist (API/data/UI items) |
| **API** | Backend only (routes, validation, errors) | Spec or request | Endpoint code, validation and error handling. No UI or schema changes. |
| **Data** | Schema and migrations only | Spec or request | Schema changes, migration files. No API handlers or UI. |
| **UI** | Frontend only (Layer 3) | Spec or request | Screens, forms, tables — content. No Layer 1–2, API, or schema. Follow `docs/agent-ui-structure-behavior-guidelines.md`. |
| **Implementation (Full-stack)** | One full feature (API + data + UI) | Issue + spec/checklist | Implement in order: define → API → data → UI. Follow `docs/application-development-guidelines.md` and UI guidelines. |
| **Test** | Unit and integration tests | Branch + code | Test code and results; hand back to Implementation or relevant role on failure. |
| **E2E** | Browser E2E | Branch + unit pass | E2E scenarios and results; fix or hand back on failure. |
| **GitHub** | PR, merge, deploy trigger | Branch + tests pass | Open PR (Closes #N), merge, (if configured) deploy. |
| **Docs** | Documentation only | Spec/code change or request | README, API docs, runbooks, etc. |
| **Review** | PR/branch review | PR or branch | Approve or request changes. |

---

## 3. Role details

### 3.1 Backlog

- **Scope**: Create issues, labels, priority, list ordering. Use the project’s backlog (e.g. GitHub issues).
- **Invoke**: “Act as Backlog Agent. Create an issue for this feature.” / “Triage open issues.”

### 3.2 Spec

- **Scope**: Define feature scope, API contract (path, request/response, errors), required data (tables/columns), and screen behavior as doc and checklist. No code.
- **Output**: Checklist that API / Data / UI or Implementation agents can follow.
- **Invoke**: “Act as Spec Agent. Write spec and checklist for login API + session storage + login screen.”

### 3.3 API Agent

- **Scope**: Add or change routes, request validation (e.g. zod), response and error shape, (project-style) auth. **Do not touch UI code or schema definitions.** If schema exists, use it for CRUD only.
- **Invoke**: “Act as API Agent. Implement only the API items from the checklist.”

### 3.4 Data Agent

- **Scope**: Change schema, add and run migrations, (optionally) seeds. **Do not touch API handlers or UI.** Only prepare tables/columns for the API agent.
- **Invoke**: “Act as Data Agent. Add user session table schema and migration.”

### 3.5 UI Agent

- **Scope**: Add or change only **Layer 3 (content/data)** — screens, forms, tables, buttons. Do not change app shell, tabs, or scroll/fill wrappers (Layer 1–2). For a new screen, copy the existing screen template or same layout pattern and fill content only. Follow `docs/agent-ui-structure-behavior-guidelines.md`.
- **Invoke**: “Act as UI Agent. Add only the login form (API already exists).”

### 3.6 Implementation (Full-stack) Agent

- **Scope**: Implement one feature in order **define → API → data → UI**. Follow `docs/application-development-guidelines.md` and UI guidelines. If the Spec agent produced a checklist, follow it.
- **Invoke**: “Act as Implementation Agent. Implement per issue #5 checklist.” / “Add this feature end-to-end.”

### 3.7 Test Agent

- **Scope**: Write and run unit/integration tests for what changed. On failure, hand back to Implementation or the relevant role or fix.
- **Invoke**: “Act as Test Agent. Test the API you just added.”

### 3.8 E2E Agent

- **Scope**: Write and run browser E2E scenarios. Can also check that layout/scroll are intact.
- **Invoke**: “Act as E2E Agent. Add and run E2E for the login flow.”

### 3.9 GitHub Agent

- **Scope**: Open PR (body includes Closes #N), merge, (if configured) trigger deploy. Does not change spec or code.
- **Invoke**: “Act as GitHub Agent. Open a PR from the current branch.”

### 3.10 Docs Agent

- **Scope**: Update README, runbooks, (optionally) API docs only. No implementation.
- **Invoke**: “Act as Docs Agent. Add local run instructions to README.”

### 3.11 Review Agent

- **Scope**: Read PR or branch and comment for compliance, bugs, or style. Does not edit; only approve or request changes.
- **Invoke**: “Act as Review Agent. Review this PR.”

---

## 4. Flow examples

### 4.1 One feature with roles

1. **Backlog** → Create issue.
2. **Spec** → Write spec and checklist for that issue.
3. **API Agent** → Implement API items from checklist.
4. **Data Agent** → (If needed) Schema and migrations.
5. **UI Agent** → Implement UI items from checklist (Layer 3 only).
6. **Test** → Run unit/integration.
7. **E2E** → Run E2E.
8. **GitHub** → Open PR and merge.

### 4.2 One agent does the full feature

1. **Backlog** → Create issue.
2. **Implementation (Full-stack)** → Implement define → API → data → UI from spec/checklist.
3. **Test** → Run tests.
4. **E2E** → Run E2E.
5. **GitHub** → Open PR and merge.

### 4.3 UI-only change

- Invoke **UI Agent** only. “Add this button.” / “Bind this field.” → Change only Layer 3.

---

## 5. Link to AGENTS.md

- **On entry**: All agents read [AGENTS.md](../AGENTS.md) first and apply the right doc (UI vs full stack) for the work scope.
- **Role invocation**: When the user says “Act as **Role** …”, follow that role’s Focus, Input, and Output above. This doc is the single source for role definitions.

---

## 6. Optional roles

| Role | Scope | Note |
|------|--------|------|
| **Security** | Dependencies, env, auth flow review | On schedule or “run security review.” |
| **Refactor** | Refactor only (no new features) | Tests must still pass. |
| **Release** | Version, channel, release steps | When the project does versioned releases. |

Add these to the table and details when the project needs them.
