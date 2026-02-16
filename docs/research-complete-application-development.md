# Complete application development — research on additional needs

This document lists items that are **not yet covered** in baro7’s AGENTS.md and docs guidelines but are commonly part of "complete application development." It suggests priorities and how to apply them.

---

## 1. What we have today (summary)

| Area | Document | Content |
|------|----------|---------|
| Entry and scope | AGENTS.md | Any agent reads AGENTS.md first; apply UI vs full-stack docs by scope |
| UI structure and behavior | agent-ui-structure-behavior-guidelines.md | Layer 1–2–3, forbidden actions, scroll/fill rules, §7 Project-specific |
| Full stack | application-development-guidelines.md | Feature flow (define→API→data→UI), Backend/API, Data, Testing, Env/security |

---

## 2. Additional items to consider (research)

### 2.1 Logging and error handling

**Content**

- **Logging**: Structured logging with request id, log level, and context. Do not log sensitive data (passwords, tokens).
- **Errors**: User-facing messages should be generic (no system details). Detailed errors are for logs and monitoring only.
- **Frontend**: Error boundaries (e.g. React) to catch exceptions, fallback UI, and retry guidance.

**Relation to our docs**  
Extends the "errors: 4xx/5xx, no sensitive info" in application-development-guidelines.md.  
**Suggestion**: Add a short "Logging and errors" section to application-development-guidelines.md — structured logging principles, user vs server message split, (optional) error boundaries.

**Priority**: High (direct impact on ops and debugging)

---

### 2.2 Security (hardening)

**Content (OWASP and common practice)**  
- Input validation and escaping (already part of API validation), logging of auth failures, permission changes, admin actions.  
- Enforce HTTPS, HSTS, security headers.  
- For public APIs, consider **rate limiting** (429, Retry-After).

**Relation to our docs**  
Env and CORS are already mentioned. Auth/permissions are only "protect per project policy."  
**Suggestion**: Add 2–3 sentences to "Environment and security" in application-development-guidelines.md — log auth/permission events, consider rate limiting for public APIs, (optional) OWASP/checklist link.

**Priority**: High (public services), Medium (internal only)

---

### 2.3 Accessibility (a11y)

**Content (WCAG 2.2 summary)**  
- Forms: label–input association (`for`/`id` or ARIA).  
- Images: `alt` when meaningful, `alt=""` when decorative.  
- Keyboard access, focus indication, semantic markup (headings, lists, tables).  
- Custom widgets: ARIA roles and state.  
- Usable at 200% zoom without horizontal scroll.

**Relation to our docs**  
UI guidelines cover layout and scroll only; a11y is not mentioned.  
**Suggestion**: Add an "Accessibility" section to agent-ui-structure-behavior-guidelines.md or application-development-guidelines.md — when adding UI/forms/buttons consider labels, semantics, keyboard, focus, alt; refer to WCAG/checklist when needed.

**Priority**: Medium (high when public or policy requires)

---

### 2.4 CI/CD and deploy

**Content**  
- On PR/push: run lint, type-check, unit and E2E tests.  
- On merge to main (or production branch): build and deploy pipeline.  
- Document or automate rollback when deploy fails.

**Example repos**  
- barocss-editor: `.github/workflows/ci.yml` (lint, type-check, unit, E2E), `docs.yml` (deploy). CI must pass before PR merge.  
- ime: `.github/workflows/deploy.yml`, Pages source = GitHub Actions.

**Relation to our docs**  
Only AGENTS.md §6 "test/verify commands" and §7 "project common rules" exist; no CI/deploy flow.  
**Suggestion**: One line in AGENTS.md §7 or application-development-guidelines.md — "Run lint/type/test in CI; deploy follows the project’s workflow (e.g. `.github/workflows/`). Document commands and paths in §7 and README."

**Priority**: High (team and PR-based development)

---

### 2.5 API extras (versioning, rate limiting, spec)

**Content**  
- **Versioning**: URL path (`/v1/`) or header. When changing, keep backward compatibility or document version.  
- **Rate limiting**: For public APIs use 429, `Retry-After`, limit headers (X-RateLimit-*).  
- **Spec**: Use OpenAPI or similar as single source for API contract and client/docs generation.

**Relation to our docs**  
API section only covers routes, validation, response, errors, auth.  
**Suggestion**: If needed, add 1–2 sentences to "Backend/API" in application-development-guidelines.md — "For public APIs consider rate limiting and versioning; an API spec (e.g. OpenAPI) helps consistency and documentation."

**Priority**: Medium (public API), Low (internal only)

---

### 2.6 Internationalization (i18n / l10n)

**Content**  
- If multiple languages are needed, set up structure early: message keys, per-locale resources, date/number formatting.  
- Do not hardcode UI strings; reference by key.

**Relation to our docs**  
Not mentioned.  
**Suggestion**: If i18n is planned, add one line in AGENTS.md §7 or application-development-guidelines.md — "i18n: If the project uses multiple languages, follow message-key and locale structure; add new copy via keys."

**Priority**: Low (single language), Medium (multi-language planned)

---

### 2.7 Performance

**Content**  
- Core Web Vitals (LCP, INP, CLS) targets.  
- Lazy loading, caching, bundle and image optimization.

**Relation to our docs**  
Not mentioned.  
**Suggestion**: For a "complete" checklist, add one line in application-development-guidelines.md or AGENTS.md — "If a new feature affects LCP/INP/CLS, measure and optimize; if the project has a performance budget, follow it."

**Priority**: Low to medium (depends on product)

---

### 2.8 Documentation

**Content**  
- README: install, env, local run, test/lint/build/deploy commands.  
- API docs: OpenAPI or equivalent spec, or summary of main endpoints.

**Relation to our docs**  
AGENTS.md §7 refers to "test/verify commands" and "README etc."  
**Suggestion**: In §7 state explicitly: "Document run/test/deploy in README and keep it updated." API docs tie in with "API extras."

**Priority**: High (onboarding and collaboration)

---

## 3. Suggested application priority

| Rank | Item | Target document | Scope |
|------|------|-----------------|-------|
| 1 | Logging and error handling | application-development-guidelines.md | One section (structured logging, user vs server messages, error boundaries) |
| 2 | CI/CD and deploy | AGENTS.md §7 or application-development-guidelines.md | 1–2 sentences + "document commands and workflow in the project" |
| 3 | Security hardening | application-development-guidelines.md (Env and security) | 2–3 sentences (auth/permission logging, rate limiting, OWASP reference) |
| 4 | Accessibility | agent-ui-structure-behavior or application-development | One section (labels, semantics, keyboard, alt, WCAG reference) |
| 5 | Documentation (README) | AGENTS.md §7 | "Document run/test/deploy in README" |
| 6 | API extras (rate limit, versioning, spec) | application-development-guidelines.md (Backend/API) | 1–2 sentences when needed |
| 7 | i18n | §7 or application-development | One line when multi-language |
| 8 | Performance | application-development or AGENTS.md | One line when needed |

---

## 4. Next steps

- **Apply soon**: Adding items 1–5 briefly to the guidelines and AGENTS.md brings the "complete application development" checklist close to done.
- **Apply optionally**: 6–8 can be added when the project has public API, i18n, or performance goals.
- **Per project**: Fill §7 Project-specific and AGENTS.md §7 with real paths, workflows, and tools (e.g. GitHub Actions, Vercel) so both agents and humans follow the same rules.
