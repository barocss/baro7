# Responsive Layout: Why and When to Use What

This doc explains breakpoints, container queries vs media queries, mobile-first, and when each approach fits so we can evolve responsive guidance without locking into one recipe.

---

## 1. Why responsive rules matter

- **Viewport diversity**: Phones, tablets, desktops, and different zoom levels mean content must reflow so it stays readable and usable. Fixed widths or desktop-only layouts fail on small screens.
- **Touch and interaction**: Small screens often imply touch; tap targets and spacing (e.g. 44px min) matter. Breakpoints are a proxy for “mobile vs desktop” even though device size and input type are not perfectly correlated.
- **Layout and readability**: Line length, stacking order, and visibility of key actions (e.g. primary button above the fold) depend on width. Responsive rules help keep structure and content effective across viewports.

So responsive guidance is about reflow, touch targets, and layout decisions by viewport, not about a single “correct” breakpoint set.

---

## 2. When to use what

- **Media queries (viewport)**: Use when layout or visibility depends on *viewport* size (e.g. “sidebar below 768px,” “two columns above 1024px”). Tailwind’s default breakpoints (`sm`, `md`, `lg`, `xl`, `2xl`) are viewport-based. Good for app shell, nav, and page-level layout.
- **Container queries**: Use when a *component* should adapt to *its container* size, not the viewport (e.g. card grid that goes 1/2/3 columns based on container width). Requires `container-type` (e.g. `inline-size`) and `@container` in CSS or Tailwind container queries. Good for reusable components that live in different-sized columns or panels.
- **Mobile-first**: Writing styles for small viewport first, then adding `min-width` breakpoints for larger screens, keeps CSS simpler and avoids over-specifying for desktop. Matches Tailwind’s default approach (unprefixed = mobile; `md:`, `lg:` = larger).

So the guideline is: viewport-dependent layout → media queries (or Tailwind breakpoints); component-dependent layout → container queries when supported; prefer mobile-first ordering.

---

## 3. Alternatives and references

- **Tailwind breakpoints**: Defaults (e.g. `sm: 640px`, `md: 768px`, `lg: 1024px`) are a good baseline; projects can override in `tailwind.config` if needed.
- **When 100% viewport is wrong**: For *content* height, avoid `100vh` (see [layout-research.md](layout-research.md)); for *width*, full viewport is common. For *internal* scroll, use flex + min-h-0, not viewport height.
- **References**: MDN (media queries, container queries), Tailwind responsive design docs, WCAG 1.4.10 Reflow (content reflow up to 320px width).

---

## 4. How this doc is used

- **Guidelines today**: [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) and SKILL.md focus on layout structure (scroll/fill, layers); they do not mandate specific breakpoints. Responsive choices are project-specific.
- **Nuance and evolution**: This doc records when to use media vs container queries and mobile-first so that when we add “responsive” bullets to the main guidelines (e.g. “use container queries for card grids”), the rule stays short and the rationale lives here.

So: we keep layout/structure rules in the main skill; we use this file for responsive strategy and for evolving breakpoint/container guidance over time.
