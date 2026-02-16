# Accessibility (a11y)

**Any AI agent** that adds or changes UI (forms, buttons, images, custom widgets, or page structure) should follow this document so the app stays accessible. Use it together with [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md) for layout and [application-development-guidelines.md](application-development-guidelines.md) for full-stack work. Rationale and references: [accessibility-research.md](accessibility-research.md). Forms detail (validation, error display): [forms-research.md](forms-research.md).

---

## 1. Principle

**Every user can perceive, operate, and understand the UI.** When adding or changing UI, do not introduce barriers for keyboard users, screen readers, or users who zoom or rely on semantics. Fix a11y regressions in the same change.

---

## 2. Forms

- **Label–input association**: Every form control must have a visible label. Use `<label for="id">` with matching `id` on the input, or wrap the input in `<label>`, or use `aria-label` / `aria-labelledby` when a visible label is not appropriate.
- **Errors and hints**: Associate validation messages and hints with the control (e.g. `aria-describedby`). Announce errors to screen readers (e.g. `aria-invalid`, live region, or focus move).
- **Required and disabled**: Use `required` and `aria-required` where applicable; use `disabled` with care and ensure disabled state is communicated.

---

## 3. Images and media

- **Meaningful images**: Provide concise `alt` text that conveys the same information or function as the image.
- **Decorative images**: Use `alt=""` and ensure the image is not focusable or announced (e.g. no role that forces announcement).
- **Complex images** (charts, diagrams): Prefer a short `alt` plus a longer description in text or `aria-describedby` to a long `alt`.

---

## 4. Keyboard and focus

- **Keyboard operable**: All interactive elements must be reachable and usable with the keyboard (Tab, Enter, Space, arrow keys as appropriate). No keyboard traps.
- **Focus visible**: Ensure focus is visible (outline or other visible focus style). Do not remove focus outline without replacing it with a clear focus indicator.
- **Focus order**: Tab order should follow a logical reading order. Use `tabIndex` only when necessary (e.g. trapping focus in a modal) and document why.
- **Skip links**: For multi-region pages, consider a “skip to main content” link at the top.

---

## 5. Semantics and structure

- **Headings**: Use a single `<h1>` per page and a logical heading hierarchy (`h2` → `h3` …). Do not skip levels for styling only; use CSS for visual hierarchy.
- **Landmarks**: Use semantic elements (`<main>`, `<nav>`, `<aside>`, `<header>`, `<footer>`, `<section>`, `<article>`) so assistive tech can navigate by region.
- **Lists**: Use `<ul>` / `<ol>` / `<li>` for lists of items. Use `<table>` with `<th>` for data tables; keep tables simple or provide summaries/captions where needed.

---

## 6. Custom widgets and ARIA

- **Roles**: Use ARIA roles (`button`, `dialog`, `tablist`, `tab`, `tabpanel`, `menu`, `menuitem`, etc.) when the element is not a native control (e.g. a div that acts as a button).
- **State and properties**: Expose state (e.g. `aria-expanded`, `aria-selected`, `aria-checked`, `aria-pressed`) and relationships (e.g. `aria-controls`, `aria-labelledby`).
- **Live regions**: Use `aria-live` (polite or assertive) for dynamic content that should be announced (e.g. toast, loading result). Prefer `role="status"` or `role="alert"` where appropriate.

---

## 7. Zoom and layout

- **Reflow**: Content should reflow and remain usable at least up to 200% zoom. Avoid horizontal scrolling for main content; use responsive layout and relative units where possible.
- **Text**: Prefer relative units (rem, em) for font size; avoid fixed pixel heights that clip text when zoomed.

---

## 8. Checklist (before shipping UI changes)

- [ ] Every form control has an associated label (or `aria-label` / `aria-labelledby`).
- [ ] Images have appropriate `alt` (meaningful text or `alt=""` for decorative).
- [ ] All interactive elements are keyboard accessible and have visible focus.
- [ ] Heading order is logical; landmarks/semantics are used.
- [ ] Custom widgets have correct roles and state (ARIA where needed).
- [ ] No a11y regressions (e.g. removed labels, focus trap, or broken semantics).

For more detail, see [WCAG 2.2](https://www.w3.org/TR/WCAG22/) and the project’s accessibility policy if one exists.
