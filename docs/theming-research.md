# Theming: CSS Variables, Dark Mode, and Design Tokens

This doc explains why theming rules exist, how CSS variables and design tokens fit in, and when to use framework defaults (e.g. shadcn) vs overrides so we can evolve theming guidance without duplicating full style guides.

---

## 1. Why theming rules matter

- **Consistency**: Colors, spacing, and typography should be consistent across the app. Central tokens (CSS variables or design-system variables) avoid magic values and make global changes (e.g. dark mode) tractable.
- **Accessibility**: Contrast and focus colors must meet WCAG. Theming should not break contrast or focus visibility; prefer semantic tokens (e.g. “primary,” “background,” “error”) over raw hex in components.
- **Maintainability**: One place to change “primary color” or “border radius” keeps the app easy to restyle and aligns with design systems (shadcn, etc.).

So theming guidance is about tokens, semantic naming, and not breaking a11y when switching themes.

---

## 2. When to use what

- **CSS custom properties (variables)**: Use for colors, spacing, radii, shadows that should change by theme or globally. Define on `:root` (or a theme class like `.dark`) and reference in components. Enables dark mode by swapping variable values.
- **Design tokens**: Same idea as variables but often defined in a config (e.g. Tailwind theme, JS object) and then emitted as CSS. shadcn/ui uses CSS variables in the stylesheet; Tailwind theme extends with those variables so utilities (`bg-background`, `text-primary`) stay consistent.
- **Framework defaults**: For React + shadcn, the default theme (e.g. `globals.css` with variable definitions) is the baseline. Override by changing variable values or adding a second theme class (e.g. `.dark`); avoid scattering hardcoded colors in components.
- **When to override**: Project-specific brand colors, compliance (e.g. required contrast), or multiple themes (light/dark/high-contrast). Document overrides in §8 (Project-specific) or a short theming section so agents know where to look.

So the guideline is: prefer semantic tokens (CSS variables or theme config); use framework defaults when present (e.g. shadcn); override in one place; keep contrast and focus visible.

---

## 3. Alternatives and references

- **shadcn/ui**: Theming is via CSS variables in `globals.css`; Tailwind’s theme extends them. See [shadcn theming](https://ui.shadcn.com/docs/theming).
- **Dark mode**: Toggle a class (e.g. `.dark`) on `html` or a wrapper and define `--background`, `--foreground`, etc. for both light and dark. Avoid mixing viewport-based media with class-based dark mode if the project wants explicit user toggle.
- **References**: MDN (custom properties), Tailwind theme configuration, WCAG contrast (1.4.3).

---

## 4. How this doc is used

- **Guidelines today**: SKILL.md and [react-shadcn-ui-7layers.md](react-shadcn-ui-7layers.md) do not mandate a full theming section; theming is often project-specific. When we add “use design tokens” or “don’t hardcode colors” to the skill, the rule stays short.
- **Nuance and evolution**: This doc records why tokens and dark mode matter and when to use framework defaults vs overrides. When we add explicit theming bullets (e.g. “override in globals.css only”), the rationale and alternatives live here.

So: we keep the skill focused on structure and layers; we use this file for theming strategy and for evolving token/dark-mode guidance over time.
