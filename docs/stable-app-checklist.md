# Stable app checklist

Use this checklist before shipping or when reviewing changes. Each pillar links to the full guide so the app does not go wrong in that dimension.

---

## 1. UI structure

- [ ] Layout, scroll, and shell (Layer 1–2 or L1–L5) were not changed for "add field" / "bind data" requests.
- [ ] New screens copy the existing screen template or same layout pattern.

**Detail**: [agent-ui-structure-behavior-guidelines.md](agent-ui-structure-behavior-guidelines.md), [layer-1-2-3-guide.md](layer-1-2-3-guide.md), [ui-7-layers.md](ui-7-layers.md). React/shadcn: [react-shadcn-ui-7layers.md](react-shadcn-ui-7layers.md).

---

## 2. API and data

- [ ] New/changed endpoints have validation, consistent response shape, and safe error messages.
- [ ] Schema changes have migrations; naming and relationships are consistent.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §2–3

---

## 3. Logging and errors

- [ ] New API/logic logs with request id and level; no secrets in logs.
- [ ] User sees safe messages; details only in server logs.
- [ ] Critical UI paths have error boundaries or equivalent.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §6

---

## 4. Security

- [ ] Inputs validated and escaped; auth/permission events logged (no secrets).
- [ ] HTTPS and security headers in place for production; public APIs consider rate limiting.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §7

---

## 5. Accessibility

- [ ] Form controls have labels; images have appropriate alt.
- [ ] Keyboard accessible; focus visible; semantics and headings logical.
- [ ] Custom widgets have correct ARIA roles and state.

**Detail**: [accessibility.md](accessibility.md)

---

## 6. CI/CD and deploy

- [ ] Lint/type/test run in CI; deploy workflow documented.
- [ ] Rollback is documented or automated.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §8

---

## 7. Performance

- [ ] No silent LCP/INP/CLS regression; large assets lazy-loaded or optimized.
- [ ] Project performance budget (if any) is respected.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §9

---

## 8. Documentation

- [ ] README documents run, test, and deploy; §7 (and README) updated when commands or paths change.

**Detail**: [application-development-guidelines.md](application-development-guidelines.md) §10, [AGENTS.md](../AGENTS.md) §7
