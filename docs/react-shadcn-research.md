# React + shadcn: Why the 7-Layer Split, and When to Use What

The baro7 [react-shadcn-ui-7layers.md](react-shadcn-ui-7layers.md) doc defines which React/shadcn pieces live in which layer and how to generate UI without breaking structure. This doc explains why we use that split, when to use Server vs Client components (Next.js), and where to look for more so we can evolve the rules with research rather than lock into one recipe.

---

## 1. Why the 7-layer split for React

- **Stable shell, changing content**: In React apps, the root, layout, nav, and scroll wrappers (L1–L5) change rarely. Buttons, forms, and data (L6–L7) change often. If agents (or developers) edit “everything” when adding a field, they risk refactoring layout, breaking scroll, or moving the client boundary in ways that hurt SSR or bundle size. The layer split keeps “do not touch” (L1–L5) and “touch here” (L6–L7) explicit.
- **shadcn and Radix**: shadcn components are composition-friendly; they don’t impose a single layout. Putting “layout” (ScrollArea, Card as container) in L4/L5/L6 and “content” (Button, Input, Table cells) in L7 avoids mixing structure and data in one layer, so generated UI stays predictable and non-breaking.
- **Next.js App Router**: With Server and Client Components, the boundary matters. Keeping L5 (page wrapper) server and L6/L7 client where needed keeps the shell small and cacheable and pushes interactivity down to where it’s needed. So the rule “L5 = server, client only in L6/L7” is a direct consequence of “structure stable, content variable.”

So the 7-layer mapping is not arbitrary: it matches how React/Next/shadcn apps are built and keeps agents from touching the wrong layer.

---

## 2. When to use Server vs Client (Next.js)

- **L2/L3/L5 as Server Components**: Root layout, nav, and page wrappers (L2, L3, L5) should stay Server Components when possible. They don’t need `useState` or browser APIs; keeping them server reduces client JS and keeps the shell stable.
- **L6/L7 with "use client"**: Use `"use client"` in components that need hooks (`useState`, `useEffect`), event handlers, or client-only libraries (e.g. React Query, form libs). That’s typically inside L6 (e.g. a Card that fetches) or L7 (forms, buttons, tables). Push the client boundary as far down as possible so only the interactive subtree is client.
- **Data loading**: Route-level loading (e.g. Next.js `loading.tsx`, or data in layout/page) can feed props into L5 children. Prefer loading at the route or page level and pass data into L6/L7 so L5 stays a dumb wrapper. When using React Query or similar in L6/L7, keep loading/error UI inside those layers so the L5 wrapper doesn’t change.

So: default to server for L2–L5; use client only where interactivity or client-only deps require it, and keep that in L6/L7.

---

## 3. Error and loading placement

- **Error boundaries**: Place at L5 (route/page) or L6 so that when a child throws, the shell (L2–L4) stays mounted and only the content area shows a fallback. Do not replace the whole app with an error screen; see [application-development-guidelines.md](application-development-guidelines.md) §6.
- **Loading and skeletons**: Render inside L6/L7 (e.g. inside Card or section), not by replacing the L5 wrapper. That way the layout (sidebar, header, scroll container) is unchanged and only the content area shows loading state.

---

## 4. References

- **React**: [React docs](https://react.dev/) (composition, lifting state). Server vs Client: [Next.js docs](https://nextjs.org/docs) (App Router, Server Components).
- **shadcn/ui**: [shadcn/ui docs](https://ui.shadcn.com/) (components, theming). [Radix UI](https://www.radix-ui.com/) (primitives; shadcn builds on them).
- **Layout**: [layout-research.md](layout-research.md) for scroll/fill and viewport; [theming-research.md](theming-research.md) for CSS variables and shadcn theming.

---

## 5. How this doc is used

- **Guidelines today**: react-shadcn-ui-7layers.md is the single source of rules (layer mapping, what to touch, layout, Next.js/client, error/loading, data). Agents follow that for generation and edits.
- **Nuance and evolution**: This doc (react-shadcn-research) records the *why*, when to use RSC vs client, and references. When we add more patterns (e.g. Remix, Vite + React), we can update the main doc with short bullets and keep the rationale here.

So: we keep the 7-layer rules in react-shadcn-ui-7layers; we use this file to back them with rationale and to evolve React/shadcn guidance over time.
