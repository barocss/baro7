# baro7

Guidelines and agent roles for building **web applications** with AI agents. Keeps UI structure (layout, scroll, shell) intact while allowing content and data changes.

## Install as a skill

Publishable on [The Agent Skills Directory](https://skills.sh/). Install with:

```bash
npx skillsadd barocss/baro7
```

## What’s in this repo

- **SKILL.md** — Main skill entry (used by skills.sh and agents). Principle, 3-layer and 7-layer UI model, feature flow, agent roles.
- **AGENTS.md** — Project entry point for any AI agent. Read first; then apply UI vs full-stack docs by scope.
- **docs/agent-ui-structure-behavior-guidelines.md** — UI layers (1–2–3), scroll/fill rules, forbidden actions. §7 for project-specific paths.
- **docs/application-development-guidelines.md** — Feature flow (define → API → data → UI), backend/API, data, testing, env/security.
- **docs/layer-1-2-3-guide.md** — Code examples, how to tell which layer, boundary cases, app situations.
- **docs/ui-7-layers.md** — OSI-inspired 7-layer UI model (L1 Viewport … L7 Content).
- **docs/agents-application-project.md** — Agent roles (Backlog, Spec, API, Data, UI, Implementation, Test, E2E, GitHub, Docs, Review).
- **docs/testing-and-verification.md** — How to verify with scenarios (no unit tests for the guidelines themselves).
- **docs/what-to-do-next.md** — Checklist: use as template only vs turn baro7 into a real app.

## Using baro7 in a new app

1. Copy **AGENTS.md** and **docs/** into your app repo.
2. Add your app code (framework, shell, API, schema).
3. Fill **AGENTS.md §7** and **docs/agent-ui-structure-behavior-guidelines.md §7** with your project paths, stack, and commands.
4. When adding features, follow the agent roles and guidelines (UI: Layer 3 only; full feature: define → API → data → UI).

## Local use

There is no app code in this repo; it’s a template and skill source. Read the docs as needed. After you initialize a real app, use the run/test/deploy commands you document in **docs/what-to-do-next.md** and **AGENTS.md §7**.
