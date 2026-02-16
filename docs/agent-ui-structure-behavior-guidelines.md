# UI Structure and Behavior Guidelines

**Any AI agent** that edits frontend, screen, or layout files in this repo should follow this document. (Installable as the **baro7** skill from [skills.sh](https://skills.sh/); see root [SKILL.md](../SKILL.md).)

---

## 1. Principle

**Data/view may change. UI behavior (scroll, fill, resize) and app structure (shell, layout, tabs) must not break.** When doing feature or data work, do not change layout, shell, or scroll structure.

---

## 2. Layer Definitions

| Layer | Contents | Agent behavior |
|-------|----------|----------------|
| **1. Core** | App shell (root, sidebar, header, tab/routing), layout primitives (e.g. ScrollRegion, FillLayout, TabContentLayout), tab list structure | Do not change for "add field" / "bind data". Change only when user explicitly asks for layout/structure change. |
| **2. Screen structure** | Per-screen layout pattern, scroll region placement, fixed header/sidebar areas, wrapper divs (flex/overflow/min-h-0) | Keep when doing data/view work. Do not remove or weaken. |
| **3. Content/data** | UI components (Button, Card, Input, Table, etc.), data binding, labels, form fields, table data | Modify here for features and data. |

**Detail**: Code examples, how to tell which layer, boundary cases, and app situations: [layer-1-2-3-guide.md](layer-1-2-3-guide.md). **UI 7-layer model** (OSI-inspired): [ui-7-layers.md](ui-7-layers.md).

---

## 3. Forbidden Actions (unless user asks for layout change)

- Remove or replace layout primitives with plain divs.
- Remove or alter scroll/fill wrappers or their flex/overflow/min-h-0 classes.
- Set content area height with `100vh` or `calc(100vh - ...)`.
- Refactor full screen layout when the request is limited (e.g. "add this button").
- Create a new screen from scratch if a screen template exists; use the template.

---

## 4. New Screen

- Use the project screen template (see Project-specific section).
- Reuse the same layout pattern as existing screens.
- Only add Layer 3 content; do not change shell or shared layout.

---

## 5. Editing a Screen

- Content/data request → change only Layer 3.
- Change only the block that contains the new/updated content; leave wrappers unchanged.
- If unsure, change only content; do not touch layout wrappers.

---

## 6. Layout Rules (reference)

- **Scroll**: Scrollable region needs `min-h-0` (flex child) and `overflow-y-auto`. Parent constrains height (e.g. `flex-1 flex flex-col min-h-0 overflow-hidden`).
- **Fill**: Filling region needs `flex-1` and in flex context `min-h-0`.
- **Do not**: `100vh` / `calc(100vh-...)` for content height.

---

## 7. Project-specific (fill in)

- **Layout primitives**: (e.g. `src/components/layout/TabContentLayout.tsx`, `ScrollRegion.tsx` — list paths and purpose.)
- **Screen template**: (e.g. `src/tabs/_TabTemplate.tsx` — path to copy for new screens.)
- **Layer 1–2 scope**: (e.g. `App.tsx` shell and tab wrapper; each tab’s top-level wrapper and sidebar/main structure — list files or roles.)

After filling this section, this doc is the single source for UI structure/behavior in this project.
