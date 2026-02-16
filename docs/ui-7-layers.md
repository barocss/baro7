# UI 7-Layer Model (OSI-Inspired)

The UI is split into **7 layers**: lower layers are "infrastructure/structure", upper layers are "content/data". Like OSI, **responsibility is separated by layer**, and it's clear **which layers may be touched** for requests like "add field".

---

## 1. Layer overview (bottom → top)

| Layer | Name | Role | Touch for "add field/button"? |
|-------|------|------|-------------------------------|
| **L1** | Viewport / Root | Viewport, document root, global style reset | No |
| **L2** | App Shell | App root layout, router container | No |
| **L3** | Chrome / Navigation | Sidebar, header, tab list, nav bars | No |
| **L4** | Layout Primitives | Shared layout components (ScrollRegion, FillLayout, etc.) | No |
| **L5** | Screen Structure | Per-screen scroll/fill wrappers (flex/overflow/min-h-0) | No |
| **L6** | Blocks / Sections | Logical blocks (card, form group, table container) | Only when adding a new section |
| **L7** | Content / Data | Buttons, inputs, labels, text, data binding, conditional UI | Yes |

- **L1–L5**: Structure and behavior (scroll, fill, shell). Do **not** change for "add field", "bind data", "change copy".
- **L6**: Touch only when **adding a block** (e.g. "add a new card section to this screen"). If the request is only to change fields inside an existing block, change L7 only.
- **L7**: Where feature, data, and copy changes happen. Modify here by default.

---

## 2. Diagram (bottom = 1, top = 7)

```
                    ┌─────────────────────────────────────┐
                    │ L7 — Content / Data                 │  ← buttons, forms, tables, data
                    │  (change on feature/data request)    │
                    ├─────────────────────────────────────┤
                    │ L6 — Blocks / Sections              │  ← cards, form groups, table wrapper
                    │  (change only when adding block)     │
                    ├─────────────────────────────────────┤
                    │ L5 — Screen Structure               │  ← per-screen scroll/fill wrapper
                    ├─────────────────────────────────────┤
                    │ L4 — Layout Primitives              │  ← ScrollRegion, FillLayout
                    ├─────────────────────────────────────┤
                    │ L3 — Chrome / Navigation            │  ← sidebar, header, tab list
                    ├─────────────────────────────────────┤
                    │ L2 — App Shell                      │  ← root layout, router
                    ├─────────────────────────────────────┤
                    │ L1 — Viewport / Root                │  ← document, viewport, reset
                    └─────────────────────────────────────┘
```

---

## 3. Per-layer detail

### L1 — Viewport / Root

- **Contains**: Browser viewport, document root (`html`/`body`), global CSS reset/base.
- **Code**: `index.html`, `_document.tsx`, reset in `globals.css`, root `div#root`, etc.
- **Agent**: Do not change for "add field"/"data binding". Only when user asks for global style/reset change.

### L2 — App Shell

- **Contains**: The app's **single root layout** and the container where the router mounts.
- **Code**: Top-level structure in `App.tsx`, `_app.tsx`, `app/layout.tsx` (one wrapper around router/Outlet).
- **Agent**: Do not change unless user asks for structure change.

### L3 — Chrome / Navigation

- **Contains**: UI where the user chooses "where to go": sidebar, top header, tab list, bottom nav.
- **Code**: `Sidebar`, `Header`, `TabList`, `NavBar` and their placement.
- **Agent**: "Add menu item" (route/tab structure change) = edit. "Change label only" can be L7. Define in project §7.

### L4 — Layout Primitives

- **Contains**: Reusable components that do **layout only** (scroll, fill). No content.
- **Code**: **Definition** of `ScrollRegion`, `FillLayout`, `TabContentLayout`.
- **Agent**: Do not change these definitions for "add field". Usage sites = L5.

### L5 — Screen Structure

- **Contains**: **Per-screen** scroll/fill structure. Uses L4 primitives or direct `flex-1`/`min-h-0`/`overflow-*` wrappers.
- **Code**: Top-level wrapper div or `<ScrollRegion>` in each `page.tsx` / `dashboard.tsx`.
- **Agent**: Do not change for content requests. Only when user asks to "change this screen's layout".

### L6 — Blocks / Sections

- **Contains**: **Logical regions** in the screen: card container, form group, section around table. Defines "what blocks exist".
- **Code**: `<Card>…</Card>`, `<section className="form-group">`, wrapper div around table.
- **Agent**: "Add field inside this card" → change **field (L7)** only; keep card (L6) structure. "Add new card section to this screen" → add new block in L6 + fill content in L7.

### L7 — Content / Data

- **Contains**: **Actual content** the user sees: buttons, inputs, labels, text, table cells, data binding, conditional UI.
- **Code**: `<Button>`, `<Input>`, `{count}`, `items.map(...)`, loading/error messages.
- **Agent**: "Add field", "add button", "data binding", "change label" → change **L7 only**.

---

## 4. Mapping to 3-layer model

| 3-layer (existing) | UI 7-layer |
|--------------------|------------|
| Layer 1 (Core) | L1 + L2 + L3 + L4 |
| Layer 2 (Screen structure) | L5 |
| Layer 3 (Content/data) | L6 + L7 |

- **"Don't touch Layer 1–2"** = **Don't touch L1–L5**.
- **"Change only Layer 3"** = **Change mainly L7**, and L6 only when adding a block.
- Keep the existing 3-layer guidelines; use the 7-layer model when you want **finer granularity**.

---

## 5. Allowed layers by request type

| Request type | Allowed layers | Note |
|--------------|----------------|------|
| Add field, data binding, add button, change label | L7 | Only add inside L6 blocks; keep L6 structure. |
| "Add new card section to this screen" | L6 + L7 | New block (L6) + content inside (L7). |
| "Change this screen layout (scroll area)" | L5 | Optionally change how L4 is used. |
| "Add menu item to sidebar" (route add) | L3 | Nav/route structure change. |
| "Add shared scroll component" | L4 | New primitive definition. |
| "Change app root layout" | L2 | Shell structure change. |
| Global style/reset change | L1 | Rare. |

---

## 6. How to use

- **Docs**: [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) stays **3-layer** by default; link to [ui-7-layers.md](ui-7-layers.md) for detail and finer layers.
- **Agents**: Default rule is "don't touch L1–L5; change only L7 (and L6 when needed)". If the project adopts 7 layers, state "add field → L7 only" explicitly.
- **Project §7**: Map layout primitives = L4, screen template = L5 pattern, Layer 1–2 scope = L1–L5 (or 3-layer L1–L2).

This gives an **OSI-style UI 7-layer** design while staying compatible with the 3-layer guidelines.
