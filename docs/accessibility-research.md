# Accessibility: Why These Rules, and What Else to Use

The baro7 [accessibility.md](accessibility.md) guidelines require labels, keyboard/focus, semantics, ARIA for custom widgets, and reflow/zoom. This doc explains why those rules exist, how they map to WCAG, when to use which pattern, and where to look for more.

---

## 1. Why these rules exist

- **Labels and forms**: Screen readers and other assistive tech need a programmatic link between a control and its purpose. Without it, users cannot complete forms reliably. Visible labels plus correct association (`for`/`id`, `aria-label`, or `aria-labelledby`) are the baseline.
- **Keyboard and focus**: Many users navigate only by keyboard or switch devices. If focus is missing, hidden, or trapped, the UI is unusable for them. Visible focus and logical tab order are required, not optional.
- **Semantics and landmarks**: Headings and landmarks (`main`, `nav`, etc.) let assistive tech users jump by region and understand structure. Div soup with no semantics forces linear reading and hides hierarchy.
- **ARIA for custom widgets**: Native controls get semantics for free; custom controls (e.g. div-as-button, custom tabs) do not. ARIA roles and state (e.g. `aria-expanded`, `aria-selected`) expose behavior so screen readers can announce it.
- **Reflow and zoom**: Fixed layouts and tiny text break for users who zoom or use large text. Reflow and relative units (rem, em) keep content usable up to 200% zoom and align with WCAG reflow criteria.

So the rules are not arbitrary: they address real barriers (forms, keyboard, structure, custom UI, zoom) that affect legal compliance and usability.

---

## 2. When these rules apply

- **All UI changes**: Any new or changed form, button, image, custom widget, or page structure should follow the a11y doc. Fix regressions in the same change.
- **When to go deeper**: Complex custom widgets (tabs, modals, menus, comboboxes) need full ARIA patterns; see [ARIA Authoring Practices Guide (APG)](https://www.w3.org/WAI/ARIA/apg/). Forms with validation need `aria-invalid`, `aria-describedby`, and live regions or focus management so errors are announced.
- **When native is enough**: Prefer native elements (`<button>`, `<input>`, `<select>`) so semantics and keyboard come for free; use ARIA when you cannot use native or are extending it.

---

## 3. WCAG mapping and references

- **WCAG 2.2**: [W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/) is the standard. Our guidelines align with Level A/AA expectations: labels (1.3.1, 4.1.2), keyboard (2.1.1), focus visible (2.4.7), headings/landmarks (1.3.1, 2.4.6), ARIA where needed (4.1.2), reflow (1.4.10).
- **ARIA APG**: [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) — patterns and examples for tabs, dialogs, menus, etc.
- **Testing**: axe DevTools, WAVE, or Lighthouse a11y audits catch many issues; manual keyboard and screen reader checks still needed for custom widgets and flow.

---

## 4. How this doc is used

- **Guidelines today**: [accessibility.md](accessibility.md) stays the single source of actionable rules so agents have a compact checklist (labels, keyboard, semantics, ARIA, zoom).
- **Nuance and evolution**: This doc (accessibility-research) records the *why*, WCAG mapping, and references. When we add patterns or exceptions (e.g. when to use `aria-live` vs focus move), we can update the main doc with short bullets and keep the detail here.

So: we keep the a11y rules as the default because they remove real barriers and align with WCAG; we use this file to back them with rationale and to evolve guidance (e.g. new ARIA patterns, project-specific overrides) over time.
