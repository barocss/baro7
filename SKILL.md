---
name: baro7
description: "Non-breaking UI (layout/scroll/shell intact) and token-optimized: compact core, load docs by scope. Use when building or editing web apps so structure never breaks and agents use fewer tokens."
---

# baro7 — Application Development & UI Structure

**Why**: (1) **Non-breaking UI** — Data and view may change; layout behavior and app structure must not break. (2) **Token use** — This skill is kept compact. Use SKILL.md as primary context; open **docs/** only for the scope you're working on (e.g. [react-shadcn-ui-7layers.md](docs/react-shadcn-ui-7layers.md) when generating React/shadcn UI, [accessibility.md](docs/accessibility.md) when adding forms, [application-development-guidelines.md](docs/application-development-guidelines.md) when doing full-stack).

---

## Principle

- **Structure (L1–L5 or Layer 1–2)**: Do not change for "add field", "bind data", "change copy". Change only when the user explicitly asks for layout/structure changes.
- **Content (L6–L7 or Layer 3)**: Change freely for features and data binding. Keep all wrappers and layout structure intact.

---

## Three-Layer Model (quick reference)

| Layer | Contents | When to touch |
|-------|----------|---------------|
| **1. Core** | App shell, sidebar, header, tab list, routing, layout primitives (ScrollRegion, FillLayout, etc.) | Only when user asks for layout/structure change. |
| **2. Screen structure** | Per-screen scroll/fill wrappers (flex-1, min-h-0, overflow-y-auto) | Do not remove or weaken when adding content. |
| **3. Content/data** | Buttons, cards, inputs, tables, labels, data binding | Modify here for features and data. |

7-layer (L1–L7) detail: [ui-7-layers.md](docs/ui-7-layers.md).

---

## Layout Rules (reference)

- **Scroll**: Scrollable region needs `min-h-0` (flex child) and `overflow-y-auto`. Parent constrains height (e.g. `flex-1 flex flex-col min-h-0 overflow-hidden`).
- **Fill**: Filling region needs `flex-1` and in flex context `min-h-0`.
- **Do not**: Use `100vh` or `calc(100vh - ...)` for content height. (Why and alternatives: [layout-research](docs/layout-research.md).)

---

## Forbidden (unless user asks for layout change)

- Remove or replace layout primitives with plain divs.
- Remove or alter scroll/fill wrappers (flex-1, min-h-0, overflow-y-auto, overflow-hidden).
- Refactor entire screen layout when the request is "add this button" or "change this field."
- Create a new screen from scratch if a screen template exists; copy the template and fill content only.

---

## Feature Flow (full stack)

When adding a feature (API + data + UI): **Define** → **API** → **Data** → **UI** (Layer 3/L7 only). New screen: copy template, fill content. For "add button" or "bind field" only: change **UI (Layer 3 / L7)** only.

---

## Agent Roles (when splitting work)

| Role | Focus |
|------|--------|
| **Spec** | Feature spec, API contract, implementation checklist. |
| **API** | Backend only (routes, validation, errors). No UI/schema. |
| **Data** | Schema, migrations only. No API handlers/UI. |
| **UI** | Frontend only, Layer 3/L7. Follow UI guidelines. |
| **Implementation** | Full feature: define → API → data → UI. |
| **Test** / **E2E** / **GitHub** / **Docs** / **Review** | As named. |

Invoke with "Act as **Role** …" and stay within that role's scope.

---

## Expert pillars (stable apps)

| Pillar | Rule | Detail |
|--------|------|--------|
| **UI structure** | L1–L5 untouched for content. | [agent-ui-structure-behavior-guidelines](docs/agent-ui-structure-behavior-guidelines.md), [layer-1-2-3-guide](docs/layer-1-2-3-guide.md), [ui-7-layers](docs/ui-7-layers.md) |
| **React/shadcn UI** | 7-layer mapping; generate L5→L6→L7 only for content. | [react-shadcn-ui-7layers](docs/react-shadcn-ui-7layers.md) |
| **API + data** | Validate, errors, schema + migrations. | [application-development-guidelines](docs/application-development-guidelines.md) §2–3 |
| **Logging and errors** | Logs + safe messages; no secrets. | [application-development-guidelines](docs/application-development-guidelines.md) §6 |
| **Security** | Auth logging, HTTPS/headers, rate limit if public. | [application-development-guidelines](docs/application-development-guidelines.md) §7 |
| **Accessibility** | Labels, semantics, keyboard, alt. | [accessibility](docs/accessibility.md) (see [accessibility-research](docs/accessibility-research.md)) |
| **CI/CD and deploy** | Lint/test in CI; deploy + rollback doc. | [application-development-guidelines](docs/application-development-guidelines.md) §8 |
| **Performance** | LCP/INP/CLS; lazy load; no regression. | [application-development-guidelines](docs/application-development-guidelines.md) §9 (see [performance-research](docs/performance-research.md)) |
| **Documentation** | README + §7 for run/test/deploy. | [application-development-guidelines](docs/application-development-guidelines.md) §10, [AGENTS.md](AGENTS.md) §7 |

---

## Project-specific

When the project has `docs/agent-ui-structure-behavior-guidelines.md` with **§8** Project-specific filled: follow those paths (layout primitives, screen template, L1–L2 scope). Otherwise infer from codebase: root/layout = Layer 1; per-screen wrappers with flex/overflow = Layer 2; everything inside = Layer 3. **To save tokens:** read SKILL.md + §7; load other docs only for the current task (UI / API / a11y / etc.).

---

## More detail

Full guides and checklists: [agent-ui-structure-behavior-guidelines](docs/agent-ui-structure-behavior-guidelines.md), [application-development-guidelines](docs/application-development-guidelines.md), [react-shadcn-ui-7layers](docs/react-shadcn-ui-7layers.md) (React/shadcn + 7 layers), [accessibility](docs/accessibility.md), [layer-1-2-3-guide](docs/layer-1-2-3-guide.md), [ui-7-layers](docs/ui-7-layers.md), [agents-application-project](docs/agents-application-project.md), [stable-app-checklist](docs/stable-app-checklist.md). Research (why/when/alternatives): [layout-research](docs/layout-research.md), [accessibility-research](docs/accessibility-research.md), [responsive-research](docs/responsive-research.md), [theming-research](docs/theming-research.md), [performance-research](docs/performance-research.md), [forms-research](docs/forms-research.md), [react-shadcn-research](docs/react-shadcn-research.md). Load by scope to save tokens.

Install: [skills.sh](https://skills.sh/) — `npx skills add barocss/baro7`
