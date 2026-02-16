# Forms: Client-Side Validation, Error Display, and a11y

The baro7 [accessibility.md](accessibility.md) §2 covers form labels, errors, and required/disabled. This doc explains why those rules exist, how client-side validation and error display fit in, and how they tie to a11y so we can evolve form guidance without duplicating the full a11y checklist.

---

## 1. Why form rules exist

- **Labels**: Every control needs a programmatic label so screen readers and other assistive tech can announce purpose. Visible labels also help all users. Missing or broken association is a top cause of form inaccessibility.
- **Errors and hints**: Validation messages must be associated with the control (e.g. `aria-describedby`, `aria-invalid`) and announced to screen readers (live region or focus move). Generic “something went wrong” at the top is not enough for multi-field forms.
- **Required and disabled**: Communicating required state (`required`, `aria-required`) and disabled state avoids confusion and failed submissions. Use disabled with care so users understand why a control is unavailable.

So the rules address both usability (clear labels, clear errors) and accessibility (programmatic association, announcements).

---

## 2. Client-side validation and error display

- **When to validate**: Client-side validation gives immediate feedback (format, required, range) and reduces unnecessary server round-trips. It does not replace server-side validation for security and data integrity.
- **When to show errors**: Inline per field (after blur or submit) is clearer than a single block at the top. Associate the message with the control (`id` + `aria-describedby`, or wrap in a live region that’s associated). On submit, focus the first invalid field and announce the error (e.g. `role="alert"` or live region).
- **Pattern**: Validate on blur and on submit; set `aria-invalid` when invalid; put the error text in an element referenced by `aria-describedby` so it’s announced. See [accessibility.md](accessibility.md) and [accessibility-research.md](accessibility-research.md) for ARIA and WCAG.
- **With React (e.g. react-hook-form + shadcn Form)**: When using form libraries with shadcn components, keep [accessibility.md](accessibility.md) rules: every control has a label (or `aria-label`/`aria-labelledby`), validation errors are associated via `aria-describedby` and `aria-invalid`, and errors are announced (e.g. via shadcn FormField + error message and focus management). Do not rely on visual-only error display.

---

## 3. Relation to accessibility and API

- **a11y**: Forms are a subset of [accessibility.md](accessibility.md). Labels, errors, required, and disabled are covered there; this doc adds the “when and how” for validation and error display.
- **API**: Server must validate and return 4xx with clear messages; client can mirror those messages and map them to fields. Do not expose sensitive server details in client messages (see application-development-guidelines §6).

---

## 4. How this doc is used

- **Guidelines today**: accessibility.md §2 is the single source for form *rules* (labels, errors, required, disabled). Agents follow that for checklist behavior.
- **Nuance and evolution**: This doc (forms-research) records client-side validation patterns, error display strategy, and the link to a11y. When we add “validate on blur,” “focus first error,” or “use aria-describedby for errors,” we can add a short bullet to accessibility.md and keep the rationale here.

So: we keep form rules in accessibility.md; we use this file for validation and error-display strategy and for evolving form guidance over time.
