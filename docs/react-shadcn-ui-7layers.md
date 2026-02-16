# React + shadcn/ui with the 7-Layer Model

Use this doc when **generating or editing UI** in a **React** app that uses **shadcn/ui**. It maps the [7-layer model](ui-7-layers.md) to concrete React/shadcn patterns so generated UI stays non-breaking and correctly layered.

---

## 1. Layer → React / shadcn mapping

| Layer | Role | React / Next / shadcn | Touch for "add field"? |
|-------|------|------------------------|------------------------|
| **L1** | Viewport, root, reset | `index.html`, `globals.css`, Tailwind base, `body` | No |
| **L2** | App shell, router | `app/layout.tsx` (Next), `_app.tsx`, `App.tsx` — single root, `<Outlet />` or `{children}` | No |
| **L3** | Chrome / nav | `Sidebar`, `NavigationMenu`, `Tabs` (list), header, nav links. shadcn: `Sidebar`, `TabsList`, `NavigationMenu`. | No (only for nav/route changes) |
| **L4** | Layout primitives | **Definition** of scroll/fill only: custom `ScrollRegion` or shadcn `ScrollArea` in a wrapper with `flex-1 min-h-0 overflow-hidden` parent. No buttons/forms inside the primitive. | No |
| **L5** | Screen structure | Each `app/**/page.tsx` or route: top wrapper `className="flex-1 flex flex-col min-h-0 overflow-hidden"` and inner `<ScrollArea className="flex-1">` or `div` with `overflow-y-auto`. | No |
| **L6** | Blocks / sections | **Container** usage: shadcn `Card` (wrapper), `CardHeader`+`CardContent`, form `<section>`, `Table` wrapper div. Defines "what blocks exist." | Only when adding a new section |
| **L7** | Content / data | shadcn `Button`, `Input`, `Label`, `Table` (rows/cells), `Badge`, `Select`, `Checkbox`, etc. + `useState`, `useForm`, data binding, conditional UI. | Yes |

---

## 2. Generating UI without breaking layers

- **Add field / button / bind data**: Change **L7 only**. Add or edit inside existing L6 blocks (e.g. inside `<CardContent>`, inside a form). Do not touch L4/L5 wrappers or L3 nav.
- **New screen**: Copy existing **L5** pattern (same wrapper + ScrollArea or scroll div), then add **L6** blocks (e.g. Cards) and **L7** content. Do not create from scratch without the L5 wrapper.
- **New section on screen**: Add **L6** (e.g. new `<Card>`) and **L7** content inside it. Do not refactor the whole page layout.
- **L4 primitive**: Define once (e.g. a `ScrollRegion` that uses `ScrollArea` + flex). Use it in L5; do not put form fields or buttons inside the L4 component definition.

---

## 3. shadcn components by layer (quick reference)

| Use in layer | shadcn / React | Notes |
|--------------|----------------|-------|
| L3 | `Sidebar`, `TabsList`, `NavigationMenu`, `Separator` (nav) | Nav/chrome only. Label copy = L7. |
| L4 | `ScrollArea` inside a flex wrapper with `min-h-0` | Layout only; children are passed in at L5. |
| L5 | `ScrollArea` or `div` with `flex-1 min-h-0 overflow-y-auto`; page root | No Card/Button/Input at this level. |
| L6 | `Card`, `CardHeader`, `CardContent`, `CardFooter`; `TabsContent` (container); form wrapper | Structure of blocks; content goes inside. |
| L7 | `Button`, `Input`, `Label`, `Select`, `Checkbox`, `Table`, `Badge`, `Dialog` (content), `Toast`, etc. | All interactive content and data binding. |

---

## 4. Layout rules (React/shadcn)

- **Scroll**: Parent that constrains height: `className="flex-1 flex flex-col min-h-0 overflow-hidden"`. Scrollable child: `ScrollArea className="flex-1"` or `div` with `min-h-0 flex-1 overflow-y-auto`. Do not use `100vh` or `calc(100vh - ...)` for content height.
- **Fill**: In a flex layout, use `flex-1` and `min-h-0` so the scroll child can shrink. shadcn `ScrollArea` works when its parent has a bounded height.
- **New page (Next.js App Router)**: In `app/your-route/page.tsx`, use the same L5 wrapper as other pages (e.g. `div` with flex + min-h-0 + overflow-hidden, then ScrollArea or scroll div). Then add L6 (Card/sections) and L7 (form controls, buttons, data).

---

## 5. Next.js, error/loading, and data

- **Server vs client**: Keep **L5** (page wrapper) as Server Components when using Next.js App Router. Use `"use client"` only in **L6/L7** (e.g. components that need `useState`, event handlers, or client-only libs). Do not push client boundary up into L4/L5 so the shell stays stable.
- **Error and loading UI**: Render error boundaries and loading/fallback UI **inside L6/L7** (e.g. inside Card or section), not by replacing the L5 wrapper. That way the app shell (L2–L5) stays intact and only content area shows retry or skeleton.
- **Data and state**: Put data fetching, React Query, or global state usage in **L6/L7** (page content, sections, forms). Keep **L4/L5** as pure structure (no fetch or state that would force layout changes). Route-level loaders (e.g. Remix, Next loaders) can feed into L5 children but should not change the L5 wrapper itself.

---

## 6. Checklist when generating UI

- [ ] L5 (page wrapper) is unchanged when adding content; L4 primitives unchanged.
- [ ] New content is inside L6 blocks (Card, section) and L7 (shadcn form/button/table).
- [ ] No Button/Input/Label placed at L4 or L5 level; they belong in L7 inside L6.
- [ ] Scroll/fill uses `min-h-0` + `flex-1` + `overflow-y-auto` (or ScrollArea); no fixed viewport height for content.

Theory and full layer rules: [ui-7-layers.md](ui-7-layers.md). General UI structure: [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md). Rationale and references: [react-shadcn-research.md](react-shadcn-research.md).
