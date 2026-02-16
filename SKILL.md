---
name: baro7
description: Application development and UI structure guidelines for AI agents. Use when building or editing web apps—preserves layout/scroll/shell (L1–L5) while allowing content changes (L6–L7). Includes 3-layer and OSI-inspired 7-layer UI model, feature flow (API → data → UI), and agent roles.
---

# baro7 — Application Development & UI Structure

Use this skill when building or editing **web applications**. Core rule: **data and view may change; layout behavior (scroll, fill, resize) and app structure (shell, tabs, wrappers) must not break.**

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

---

## UI 7-Layer Model (OSI-inspired)

For finer granularity:

| Layer | Name | Touch for "add field"? |
|-------|------|------------------------|
| L1 | Viewport / Root | No |
| L2 | App Shell | No |
| L3 | Chrome / Navigation | No |
| L4 | Layout Primitives | No |
| L5 | Screen Structure | No |
| L6 | Blocks / Sections | Only when adding a new section/block. |
| L7 | Content / Data | Yes |

**Rule**: For "add field", "add button", "bind data" — modify **L7 only** (and L6 only when adding a new block). Do not change L1–L5.

---

## Layout Rules (reference)

- **Scroll**: Scrollable region needs `min-h-0` (flex child) and `overflow-y-auto`. Parent constrains height (e.g. `flex-1 flex flex-col min-h-0 overflow-hidden`).
- **Fill**: Filling region needs `flex-1` and in flex context `min-h-0`.
- **Do not**: Use `100vh` or `calc(100vh - ...)` for content height.

---

## Forbidden (unless user asks for layout change)

- Remove or replace layout primitives with plain divs.
- Remove or alter scroll/fill wrappers (flex-1, min-h-0, overflow-y-auto, overflow-hidden).
- Refactor entire screen layout when the request is "add this button" or "change this field."
- Create a new screen from scratch if a screen template exists; copy the template and fill content only.

---

## Feature Flow (full stack)

When adding a feature (API + data + UI):

1. **Define** — What to store/expose; schema/API spec if needed.
2. **API** — Endpoints, request/response, validation, errors. Follow project route location.
3. **Data** — Schema + migration if new tables/columns; then use in API.
4. **UI** — Layer 3 (or L7) only. New screen: copy existing screen template or same layout pattern, then fill content.

For "add button" or "bind field" only: change **UI (Layer 3 / L7)** only.

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

## Project-specific

When the project has `docs/agent-ui-structure-behavior-guidelines.md` with §7 filled: follow those paths (layout primitives, screen template, L1–L2 scope). Otherwise infer from codebase: root/layout = Layer 1; per-screen wrappers with flex/overflow = Layer 2; everything inside = Layer 3.

---

## More detail

Full guidelines, code examples, boundary cases, and app situations (sidebar, tabs, modal, master-detail, etc.) are in this repo under **docs/**:

- [docs/agent-ui-structure-behavior-guidelines.md](docs/agent-ui-structure-behavior-guidelines.md)
- [docs/application-development-guidelines.md](docs/application-development-guidelines.md)
- [docs/layer-1-2-3-guide.md](docs/layer-1-2-3-guide.md)
- [docs/ui-7-layers.md](docs/ui-7-layers.md)
- [docs/agents-application-project.md](docs/agents-application-project.md)

Install this skill from [skills.sh](https://skills.sh/): `npx skillsadd barocss/baro7`
