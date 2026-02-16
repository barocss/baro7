# baro7 — What to Do Next

baro7 currently has **agent guidelines and roles** only; **app code and §7** are empty. Below is a task list by goal.

---

## What’s in the repo

- `AGENTS.md` — Entry point, UI/full-stack docs, agent roles
- `docs/agent-ui-structure-behavior-guidelines.md` — UI layers, forbidden actions, layout rules, §7 (empty)
- `docs/application-development-guidelines.md` — Feature flow, API, data, testing, env/security
- `docs/agents-application-project.md` — Backlog, Spec, API, Data, UI, Implementation, Test, E2E, GitHub, Docs, Review
- `docs/research-complete-application-development.md` — (Reference) Logging, CI, a11y, etc.

---

## A. Using as template only (copy into another app)

| # | Task | Description | Status |
|---|--------|------|--------|
| 1 | **README.md** | Short note: this repo is a web app agent guide and role template; copy AGENTS.md and docs/ into a new app, add code, then fill §7. | Done |
| 2 | **(Optional) .gitignore** | Common Node/artifact ignores for app init. | Done |

§7 and app code are filled in the **project that copies** baro7.

---

## B. Turning baro7 into a real app

### Step 1: Project skeleton

| # | Task | Description |
|---|--------|------|
| 1 | **Choose stack** | e.g. Next.js + React + Tailwind + shadcn/ui. Document in AGENTS.md §7 “Stack”. |
| 2 | **Init repo** | `package.json`, package manager (pnpm recommended), install deps. Fill §7 “Package manager”. |
| 3 | **README.md** | Local run (`pnpm dev` etc.), test/lint/typecheck commands, (if any) deploy. Align with §7 “Test/verification commands”. |
| 4 | **.env.example** | List required env keys (e.g. `DATABASE_URL`, `API_KEY`). Real values in `.env` (git-ignored). |
| 5 | **.gitignore** | node_modules, .env, build output, etc. |

### Step 2: App structure (Layer 1–2)

| # | Task | Description |
|---|--------|------|
| 6 | **App shell** | Root layout, (if needed) sidebar, header, tabs/routing. UI guideline Layer 1. |
| 7 | **Layout primitives** | One scroll region and one fill region component (e.g. `ScrollRegion`, `FillLayout`). |
| 8 | **First screen template** | One screen using that pattern. Later screens copy it and fill Layer 3 only. |
| 9 | **Fill docs/agent-ui-structure-behavior-guidelines.md §7** | Layout primitives path, screen template path, Layer 1–2 scope (files/roles). |

### Step 3: API and data (if using a backend)

| # | Task | Description |
|---|--------|------|
| 10 | **API route location** | e.g. `src/app/api/`. Fill AGENTS.md §7 “API route path”. |
| 11 | **Schema / migration location** | e.g. `src/lib/schema.ts`, `drizzle/`. Fill §7 “Schema/migration path”. |
| 12 | **(Optional) CI** | `.github/workflows/ci.yml` — lint, typecheck, test on PR. |

### Step 4: Verification and docs

| # | Task | Description |
|---|--------|------|
| 13 | **Test/lint commands** | Define `pnpm test`, `pnpm lint`, `pnpm typecheck`, etc. Document in README and §7. |
| 14 | **Fill AGENTS.md §7** | Package manager, stack, API path, schema path, test commands, (if any) branch/commit rules. |

---

## C. General

| # | Task | Description |
|---|--------|------|
| - | **Use issues for next work** | When building a real app, create issues (e.g. via Backlog agent) and run Spec → Implementation (or API/Data/UI) → Test → E2E → GitHub. |
| - | **Split by role** | “Act as Spec Agent …”, “Act as UI Agent …” etc. from `docs/agents-application-project.md` keep scope clear. |

---

## Summary

- **Template only**: README and (optional) .gitignore are enough.
- **Turn baro7 into a real app**: 1) Choose stack, init repo, README, env, gitignore → 2) App shell, layout primitives, first screen template → 3) Fill UI guideline §7 → 4) Set API/data paths and fill §7 (and optional CI) → 5) Add features using agent roles and guidelines.

You can tick off or mark “Done” as you go.
