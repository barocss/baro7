# Testing and verification — baro7 template and skill

baro7 is a **guideline and role template with no app code**; the skill is **agent instructions, not runnable code**. So "testing" means **scenario-based verification**, not unit tests.

---

## 1. How to verify the baro7 template

### 1.1 Current state (no app code)

- There are **no unit tests** (e.g. `pnpm test`). Verification is done in two ways.

**Method A: Document and structure verification**

- Check that AGENTS.md, docs/ guidelines, and agent role docs **exist**.
- Check that §7 and path placeholders are **in a form that can be filled in**.
- When copying into a new app, check that the copy → fill §7 flow works **as README describes**.

**Method B: Agent scenario verification**

- Use a **test app** (e.g. a minimal app copied from baro7 or another repo).
- In that app, give the agent requests like the ones below and **have a human confirm** that the result followed the guidelines.
  - Example: "Add a button to this screen" → confirm only Layer 3 was changed and scroll/fill wrappers were not touched.
  - Example: "Act as UI Agent. Bind data to this field" → confirm only the UI Agent scope was changed.

### 1.2 When baro7 becomes a real app later

- **App code** testing: run the project’s unit/integration/E2E tests. (Document commands in AGENTS.md §7 and README.)
- **Guideline compliance** verification: run "Method B" periodically or add items like "No Layer 1–2 changes" to the PR checklist.

---

## 2. Can the skill be tested?

### 2.1 What the skill is

- The skill (e.g. `ui-structure-behavior`, `application-development`) is **markdown instructions**. It’s not code but rules: "when you get this kind of request, behave like this."
- When Cursor (or another platform) **feeds the skill to the agent as context**, the agent follows those rules.
- So there is **no test runner that executes the skill**. Verification is: does the "skill-enabled agent" **behave correctly in given scenarios**?

### 2.2 Skill "testing" = scenario verification

- **Scenario**: define (project state + user request) → **expected behavior**.
- **Verification**: give the agent the request and check that the output (code changes, explanation) matches the expected behavior—**by human review** or **simple automated checks** (e.g. diff to ensure certain files/lines did not change).

Example (ui-structure-behavior):

| Scenario (request) | Expected behavior | How to verify |
|--------------------|-------------------|---------------|
| "Add a submit button to this screen" | Only Layer 3 changed. No change to divs/components that have scroll/fill wrappers (min-h-0, flex-1, overflow, etc.). | In the diff, confirm layout wrapper files did not change. |
| "Create a new screen" | Copy existing screen template (or same layout pattern), then fill content only. Do not change shell/tab structure. | Confirm the new file uses the same wrapper structure as the template. |
| "Bind a value to this input field" | Only Layer 3 (form/binding) changed. | Confirm layout-related classes/components were not removed or changed. |

Example (application-development):

| Scenario (request) | Expected behavior | How to verify |
|--------------------|-------------------|---------------|
| "Add login API + session storage + login screen" | Order: define → API → data → UI. UI touches only Layer 3. | Confirm API, schema, and UI were added in the right places and UI did not touch Layer 1–2. |
| "Add request validation (zod) to this endpoint" | Only the API layer changed. No change to UI or schema. | Confirm changes are limited to API route/handler. |

### 2.3 Reusing scenarios

- Keep a **scenario list** in a doc. (§3 below has examples.)
- Optionally use a **test repo** and run the same scenarios there periodically; use diff/screenshots/checklist for regression: "with the skill applied, does behavior stay correct?"
- This is not Cursor-only: with **any AI agent**, say "read this project’s AGENTS.md and docs/ (or the same content as the skill) and run this scenario," then verify the result.

---

## 3. Scenario examples (copy and use)

Below are **prompt examples** you can give to the agent and then verify the result. Assume the project has screens and APIs.

### UI structure and behavior (ui-structure-behavior / agent-ui-structure-behavior-guidelines)

1. **"Add a [Submit] button to this page. Do not touch the existing layout or scroll structure."**  
   - Verify: Layout wrappers (e.g. divs with `min-h-0`, `overflow-y-auto`, `flex-1`) were not modified or removed.

2. **"Add an email field to this form and bind its value to state."**  
   - Verify: Only Layer 3 (form/state) was added or changed; wrapper structure is unchanged.

3. **"Create a new [Settings] screen. Use the same layout pattern as the existing [Home] screen."**  
   - Verify: The new screen uses the same scroll/fill wrapper structure as the existing screen; shell/tabs are unchanged.

### Full stack (application-development / application-development-guidelines)

4. **"Act as Spec Agent. Produce a spec and implementation checklist for [user list] feature."**  
   - Verify: API, data, and UI items are clearly separated.

5. **"Act as API Agent. Implement only the API items from the checklist above."**  
   - Verify: Only API routes, validation, and errors were added; UI and schema files were not touched.

6. **"Act as UI Agent. Implement only the UI items from the checklist above. Do not modify layout wrappers."**  
   - Verify: Only Layer 3 was added or changed.

### Agent roles (agents-application-project)

7. **"Act as Implementation Agent. Implement issue #N according to its checklist in [define → API → data → UI] order."**  
   - Verify: Order was followed and Layer 1–2 was preserved in the UI.

---

## 4. Summary

| Target | How to test |
|--------|-------------|
| **baro7 template** | Check that docs and structure exist. (Optional) Run agent scenarios in a test app and verify guideline compliance. |
| **Skill** | There is no "test runner." Define scenarios (request + expected behavior), give the request to the agent, then verify the result by human review or diff/checklist. |
| **Real app (when using baro7 as an app)** | Run the project’s test commands (unit/E2E). Use the scenario verification above to check guideline compliance. |

The skill and baro7 guidelines encode **the same rules**, so "skill testing" and "baro7 guideline testing" can be done in one go with the same scenarios.

---

## 5. How to run scenario verification (step by step)

### 5.1 Preparation

1. **Choose what to verify**  
   - Example: "I want to check that an agent using the UI guideline (or ui-structure-behavior skill) only changes Layer 3."

2. **Prepare a test project**  
   - **Option A**: A **minimal app** copied from baro7 (shell + layout wrapper + one screen).  
   - **Option B**: An **existing web app** that already has a layout structure.  
   - In that project, **write down what must not be touched**.  
     - Example: `src/components/layout/ScrollRegion.tsx` must not be modified.  
     - Example: The div with `className="flex-1 min-h-0 overflow-y-auto"` in `src/app/dashboard/page.tsx` must not be removed or changed.

3. **(Optional) Save a baseline**  
   - Create a clean state with `git commit`, then run the scenario.  
   - After verification, use `git diff` to see what changed.

### 5.2 Run the scenario

1. **Give the agent context**  
   - Cursor: the skill may be attached automatically, or open a project that has AGENTS.md and docs/ so the agent reads them.  
   - Other agents: say "Read this project’s AGENTS.md and docs/agent-ui-structure-behavior-guidelines.md and then do the task," or put that content in the system prompt.

2. **Send the scenario prompt**  
   - Example: "Add a [Submit] button to this page. Do not touch the existing layout or scroll structure."  
   - When the agent has changed the code, proceed to the next step.

### 5.3 Verification (check results)

1. **See what files changed**  
   - Use `git status` and `git diff` to list and inspect changes.

2. **Check that only expected things changed**  
   - **UI scenario (add button/field)**  
     - **Expected**: Only **content (Layer 3)** such as buttons, form, state was added or changed.  
     - **Check**: If wrapper divs/components with `min-h-0`, `flex-1`, `overflow-y-auto`, `overflow-hidden` appear in the diff, ensure the change is not "deleted or classes removed." If anything was removed, **fail**.  
   - **API scenario (API-only change)**  
     - **Expected**: Only API routes and handlers changed.  
     - **Check**: UI components and schema files must not appear in the diff. If they do, **fail**.

3. **(Optional) Use a checklist**  
   - Example:

   ```
   [ ] Are changed files within the expected scope? (e.g. only screen file, or only API route)
   [ ] Were Layer 1–2 (shell, tabs, scroll/fill wrappers) preserved (not removed or weakened)?
   [ ] Was the requested feature (button/field/API) actually added or updated?
   ```

   - If any answer is no, record the scenario as **failed**.

### 5.4 Repetition and automation (optional)

- **Running the same scenario multiple times**: In preparation, use a "baseline branch"; each time run the scenario from that branch, check the diff, and record pass/fail.  
- **Automation**: You can script rules like "if this directory/file appears in the diff, fail."  
  - Example: `scripts/verify-no-layout-changes.sh` exits with 1 if `git diff` shows removal of `src/components/layout/` or `min-h-0`.  
- Full automation is difficult; **human review of diff + checklist** is usually the way.

### 5.5 One-line summary

**Prepare (test app + list of things that must not be touched) → send scenario prompt to agent → inspect changes with git diff → verify with checklist that Layer 1–2 is preserved and only the requested changes were made.**
