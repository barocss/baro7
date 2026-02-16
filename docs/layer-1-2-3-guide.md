# Layer 1·2·3 — Detailed Guide

This doc extends [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) with **code examples, how to tell which layer, boundary cases, and app situations**. Use it when deciding “is this Layer 1, 2, or 3?”.

---

## 1. Structure overview (diagram)

```
┌─────────────────────────────────────────────────────────────────┐
│ Layer 1 — Core                                                   │
│  (App shell: root, sidebar, header, tab list, routing, layout   │
│   primitives)                                                    │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ Layer 2 — Screen structure                                   │ │
│  │  (Per-screen: scroll region wrappers, flex/overflow/min-h-0)   │ │
│  │  ┌───────────────────────────────────────────────────────┐ │ │
│  │  │ Layer 3 — Content/data                                 │ │ │
│  │  │  (Buttons, cards, forms, tables, labels, data binding) │ │ │
│  │  └───────────────────────────────────────────────────────┘ │ │
│  └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

- **Layer 1**: App-wide, defined once. “With this structure, the app runs.”
- **Layer 2**: **Layout wrappers** repeated per screen (page/tab). Responsible for scroll and fill.
- **Layer 3**: **Actual content** inside. Only this is modified for feature/data requests.

---

## 2. Layer in code examples

### 2.1 Layer 1 example (Core)

**Role**: App root, sidebar/header, tab/route list, shared layout components.

```tsx
// App.tsx (or layout.tsx, _app.tsx) — Layer 1
export default function App() {
  return (
    <div className="flex h-screen overflow-hidden">
      <Sidebar />                    {/* Layer 1: global nav */}
      <main className="flex-1 flex flex-col min-h-0">
        <Header />                  {/* Layer 1: global header */}
        <TabList />                 {/* Layer 1: tab/route list */}
        <Outlet />                  {/* inside = per-screen Layer 2 → 3 */}
      </main>
    </div>
  );
}
```

- **Layout primitives** (definition) are also Layer 1.

```tsx
// components/layout/ScrollRegion.tsx — Layer 1 (primitive)
export function ScrollRegion({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex-1 flex flex-col min-h-0 overflow-hidden">
      <div className="flex-1 min-h-0 overflow-y-auto">{children}</div>
    </div>
  );
}
```

- Do **not** modify these files for “add field” or “add button” requests. Only when the user asks for layout changes.

---

### 2.2 Layer 2 example (Screen structure)

**Role**: **Wrappers around content** in each screen (page/tab). divs (or layout component usage) that use `flex-1`, `min-h-0`, `overflow-*` for scroll/fill.

```tsx
// pages/dashboard.tsx (or app/dashboard/page.tsx) — Layer 2 here
export default function DashboardPage() {
  return (
    <div className="flex-1 flex flex-col min-h-0 overflow-hidden">
      {/* ↑ This whole div is Layer 2: screen scroll/fill wrapper */}
      <div className="flex-1 min-h-0 overflow-y-auto p-4">
        {/* ↑ This div is also Layer 2: actual scroll area */}
        <h1>Dashboard</h1>
        <StatsCards />
        <RecentTable />
        {/* ↑ All above are Layer 3 */}
      </div>
    </div>
  );
}
```

- If the project has a primitive like **ScrollRegion**, using it here is Layer 2.

```tsx
export default function SettingsPage() {
  return (
    <ScrollRegion>
      {/* ScrollRegion usage = Layer 2. Inside = Layer 3 */}
      <h1>Settings</h1>
      <SettingsForm />
    </ScrollRegion>
  );
}
```

- “Add a button to this page” → add **Layer 3 only**. Do not touch the `div` or `ScrollRegion` above.

---

### 2.3 Layer 3 example (Content/data)

**Role**: User-visible UI elements, data binding, form/table/card content.

```tsx
// When modifying only Layer 3
<div className="flex-1 min-h-0 overflow-y-auto p-4">
  <h1>Dashboard</h1>
  <div className="flex gap-4">
    <Card><span>Users {count}</span></Card>
    <Card><span>Revenue {revenue}</span></Card>
  </div>
  <Button onClick={onSubmit}>Save</Button>
  <Table data={items} />
</div>
```

- `Card`, `Button`, `Table`, `h1`, text, state binding — all Layer 3. **Add or edit freely here.**
- Do not remove or strip classes from **that div (flex-1 min-h-0 overflow-y-auto)**; it is Layer 2.

---

## 3. How to tell which layer

Use this order:

1. **App-wide, defined once, and defines routing/tabs/sidebar/header?**  
   → **Layer 1.** (e.g. App.tsx, layout.tsx, Shell.tsx)

2. **Reusable layout-only component?** (ScrollRegion, FillLayout, TabContentLayout, etc.)  
   → **Layer 1.** (The definition file. Where it’s used is Layer 2.)

3. **Inside a screen (page/tab) file and is a “height/scroll only” wrapper?**  
   - Has at least one of `flex-1`, `min-h-0`, `overflow-hidden`, `overflow-y-auto`, and  
   - Contains “actual content (buttons, forms, tables)”  
   → **Layer 2.**

4. **Buttons, forms, tables, cards, text, data binding inside that?**  
   → **Layer 3.**

5. **If unclear**  
   → Treat as **Layer 3** and modify only that. Do not remove or weaken wrappers that look like Layer 2.

---

## 4. Boundary cases

| Case | Usually | Notes |
|------|---------|-------|
| **Card component** | Layer 3 | Card wrapping content. Changing content/count = Layer 3. |
| **Whole screen wrapped in one Card** | If Card acts as scroll wrapper → closer to Layer 2 | If used only for layout with flex/overflow, treat as Layer 2 and don’t remove. |
| **Modal / Drawer** | Content = Layer 3. Container (backdrop, position) = L1 or L2 by project | Form/buttons inside modal = Layer 3. “Change modal layout” = L1–L2. |
| **Table header / paging UI** | Layer 3 | Part of table. Change on data/feature request. |
| **Sidebar menu item list** | Layer 1 | Part of tab/route list. “Add menu item” (route change) = 1; “change label only” can be 3. Define in project §7. |
| **Empty state / Loading skeleton** | Layer 3 | One state of content. Add via conditional render. |

- **Principle**: For “change only data/view” requests, change **Layer 3 only**. Touch 1–2 only when the user asks for layout/scroll/shell changes.

---

## 5. App situations

### 5.1 SPA with sidebar (dashboard-style)

- **Layer 1**: Root layout, sidebar, header, tab/route list, `<main>`, etc.
- **Layer 2**: Each page’s “main content area” wrapper. `flex-1 min-h-0 overflow-y-auto` or ScrollRegion.
- **Layer 3**: Cards, tables, forms, buttons inside.
- **Nested scroll**: If the sidebar scrolls, its inner scroll area is still part of Layer 1. Main area scroll = Layer 2. Do not remove either wrapper.

### 5.2 Full-screen only (no sidebar)

- **Layer 1**: Root, (if any) top header, routing.
- **Layer 2**: Each page’s single scroll wrapper (one div or ScrollRegion).
- **Layer 3**: Page content.

### 5.3 Tabbed screen (only tab content scrolls)

- **Layer 1**: App shell + what renders the tab list.
- **Layer 2**: Scroll wrapper **inside the tab panel**. Same wrapper structure when switching tabs.
- **Layer 3**: Per-tab content. “Add field to tab” → change only that tab’s Layer 3.

### 5.4 Master–detail (list + detail panel)

- **Layer 1**: App shell.
- **Layer 2**: List area wrapper + detail area wrapper (each keeps flex/overflow). Both are “screen structure” = 2.
- **Layer 3**: List items, detail form/buttons.

### 5.5 Auth layout vs main layout

- **Login/signup** with a different shell: that layout is also **Layer 1** (a separate “shell”). Keep it separate from the main app shell.
- **Request**: “Add field to login form” → do not touch the login shell; change only the **form (Layer 3)**.

### 5.6 Modal / drawer

- **Form/buttons/text inside modal**: Layer 3. “Add input to modal” → change modal **content** only.
- **Modal container** (position, backdrop, size): If the project treats it as layout, Layer 1 or 2. Change only when “change modal layout” is requested.

### 5.7 No layout primitives (plain divs only)

- **Layer 1**: Files/components that render root, sidebar, header, routing.
- **Layer 2**: **Any div** that plays the “scroll/fill” role with `flex-1`, `min-h-0`, `overflow-y-auto`, etc. Do not remove or strip its classes.
- **Layer 3**: Everything inside.
- If the component isn’t named ScrollRegion but has the **same role**, treat it as Layer 2.

### 5.8 Next.js App Router vs Pages

- **App Router**: `app/layout.tsx` = Layer 1. Scroll wrapper inside `app/dashboard/page.tsx` = Layer 2. Inside = Layer 3.
- **Pages**: `_app.tsx` = Layer 1. Wrapper inside `pages/dashboard.tsx` = Layer 2. Inside = Layer 3.
- **Same rules;** only file paths differ.

### 5.9 Vue / other frameworks

- **Layer 1**: Root layout, navigation, router view.
- **Layer 2**: Scroll/fill wrapper inside each view (page), same Tailwind or framework styles.
- **Layer 3**: Content. Same principles and criteria.

---

## 6. Summary

| | Layer 1 | Layer 2 | Layer 3 |
|--|---------|---------|---------|
| **What** | App shell, tabs/routing, layout primitives | Per-screen scroll/fill wrappers | Buttons, forms, tables, data |
| **When to touch** | Only on layout/structure change request | Same | On feature/data request |
| **In code** | App, layout, Sidebar, ScrollRegion definitions | flex-1/min-h-0/overflow wrappers in pages | All content inside |
| **Situations** | With/without sidebar, auth shell, tab list = 1 | If it’s the scroll area, even a div = 2 | Modal content, empty state, loading = 3 |

- **“Add field/button”, “data binding”** → change Layer 3 only.
- **“Create new screen”** → copy existing screen’s Layer 2 pattern, then fill Layer 3 only.
- **If unclear** → change only Layer 3; do not remove or weaken wrappers (Layer 2).

Use this guide with [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md). Fill project paths and component names in that doc’s §7 Project-specific.

**UI 7-layer model**: For finer structure, see [ui-7-layers.md](ui-7-layers.md) (OSI-inspired L1 Viewport … L7 Content). 3-layer Layer 1 = L1–L4, Layer 2 = L5, Layer 3 = L6–L7.
