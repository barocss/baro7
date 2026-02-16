# baro7

Guidelines and agent roles for building **web applications** with AI agents. baro7 is built for **non-breaking UI** (layout/scroll/shell intact) and **token-optimized** skill use (compact SKILL.md; load docs by scope).

## Install as a skill

Publishable on [The Agent Skills Directory](https://skills.sh/). Install with:

```bash
npx skillsadd barocss/baro7
```

## What’s in this repo

- **SKILL.md** — Main skill entry (used by skills.sh and agents). Principle, 3-layer and 7-layer UI model, feature flow, agent roles, and expert pillars (logging, security, a11y, CI/CD, performance, docs).
- **AGENTS.md** — Project entry point for any AI agent. Read first; then apply UI vs full-stack docs by scope.
- **docs/agent-ui-structure-behavior-guidelines.md** — UI layers (1–2–3), scroll/fill rules, forbidden actions, accessibility link. §8 for project-specific paths.
- **docs/application-development-guidelines.md** — Feature flow, backend/API, data, testing, logging and errors (§6), security (§7), CI/CD and deploy (§8), performance (§9), documentation (§10).
- **docs/accessibility.md** — Accessibility guide: forms, images, keyboard/focus, semantics, ARIA, WCAG-aligned checklist.
- **docs/stable-app-checklist.md** — Master checklist for all pillars (UI, API, logging, security, a11y, CI/CD, performance, docs) before shipping.
- **docs/layer-1-2-3-guide.md** — Code examples, how to tell which layer, boundary cases, app situations.
- **docs/ui-7-layers.md** — OSI-inspired 7-layer UI model (L1 Viewport … L7 Content).
- **docs/layout-research.md** — Why the default layout rule (flex + min-h-0 + overflow) exists, when it applies, and research on alternatives (Grid, viewport units, etc.).
- **docs/accessibility-research.md** — Why a11y rules exist, WCAG mapping, when to use which pattern, references.
- **docs/responsive-research.md** — Breakpoints, container queries vs media queries, mobile-first; when to use what.
- **docs/theming-research.md** — CSS variables, dark mode, shadcn/design tokens; default vs overrides.
- **docs/performance-research.md** — Frontend: why LCP/INP/CLS, when to lazy-load/code-split, bundle strategies.
- **docs/forms-research.md** — Client-side validation patterns, error display, a11y link.
- **docs/react-shadcn-ui-7layers.md** — React + shadcn/ui mapped to 7 layers; use when generating or editing React/shadcn UI so structure stays intact.
- **docs/react-shadcn-research.md** — Why the 7-layer split for React/shadcn, when to use Server vs Client (Next.js), error/loading placement, references.
- **docs/agents-application-project.md** — Agent roles (Backlog, Spec, API, Data, UI, Implementation, Test, E2E, GitHub, Docs, Review).

## Using baro7 in a new app

1. Copy **AGENTS.md** and **docs/** into your app repo.
2. Add your app code (framework, shell, API, schema).
3. Fill **AGENTS.md §7** and **docs/agent-ui-structure-behavior-guidelines.md §7** with your project paths, stack, and commands.
4. When adding features, follow the agent roles and guidelines (UI: Layer 3 only; full feature: define → API → data → UI).

## Local use

There is no app code in this repo; it’s a template and skill source. Read the docs as needed. After you initialize a real app, use the run/test/deploy commands you document in **AGENTS.md §7**.
