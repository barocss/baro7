# Layout: Why This Pattern, and What Else Exists

The baro7 guidelines recommend **flex-1 + min-h-0 + overflow-y-auto** for scroll/fill and **avoid 100vh for content height**. This doc explains why that pattern is used by default, when it applies, and what other layout approaches exist so we can evolve the rules with research rather than treat one recipe as universal.

---

## 1. Why this pattern exists (technical)

- **Flex and scroll**: In flexbox, a flex child has an implicit `min-height: auto`, so it will not shrink below its content height. That means `overflow-y: auto` never gets a chance to create a scrollbar—the container just grows. Setting **min-h-0** (or `min-height: 0`) allows the child to shrink, so the parent can constrain height and the inner area can scroll.
- **flex-1**: So the scroll/fill region takes remaining space instead of a fixed height.
- **Parent**: The parent of the scrollable area must constrain height (e.g. `flex-1 flex flex-col min-h-0 overflow-hidden`) so the scroll child has a bounded height. Otherwise the scroll child expands and no scrolling happens.
- **100vh and content**: Using `100vh` (or `calc(100vh - ...)`) for the main content area is problematic on mobile (address bar show/hide changes viewport height, causing jump or double scroll), and it often fights with flex layouts. So the guideline says: avoid 100vh for content; use flex + min-h-0 so the scroll region is “fill remaining space” instead of “exactly viewport height.”

So the rule is not arbitrary: it fixes a concrete flexbox + overflow behavior and avoids common viewport-height pitfalls. That’s why it’s repeated as the **default** in SKILL.md and the UI guidelines.

---

## 2. Why it’s “default” rather than “only”

- **Context-dependent**: Full-height app shells (sidebar + main content, tabs) benefit most. Marketing pages, dashboards with a fixed chrome, or wizards often fit this. Other UIs (e.g. long single-column pages, or layouts that don’t need an internal scroll) may not need this exact pattern.
- **Other layout models**: CSS Grid, `dvh`/`svh`/`lvh`, container queries, or a mix of flex and grid can be better in some cases. The current rule is optimized for **flex-based app shells with an internal scroll region**, not for every layout on the web.
- **Frameworks and components**: Next.js App Router, shadcn ScrollArea, or custom layout primitives might wrap this pattern; the *concept* (bounded height + scroll inside) stays, the classes or components can vary.

So the guideline is a **strong default** for “app with shell + scrollable content,” not a law for all layouts. More research helps us say when to use what.

---

## 3. What “layout research” could cover

- **When this rule applies**: e.g. “Shell + main content area,” “tabs with scroll per tab.” When it doesn’t: e.g. single full-page scroll, no chrome.
- **Alternatives**:
  - **Grid**: `grid-template-rows: auto 1fr` (or similar) for header + main; main can use `min-height: 0` and `overflow-y: auto` in the grid cell for the same effect.
  - **Viewport units**: `dvh`/`svh`/`lvh` (dynamic/small/large viewport) for cases where a true viewport height is needed; tradeoffs (mobile bar, support).
  - **Container queries**: For components that need to adapt to their container rather than the viewport.
  - **When 100vh is acceptable**: e.g. one-off full-screen sections or modals where the chrome is not a flex sibling; document the exception rather than “never.”
- **References**: MDN (flex, min-height, overflow), W3C viewport units, Tailwind/shadcn layout docs, or internal app patterns.
- **Project-specific overrides**: In §8 (Project-specific), a project could state “we use Grid for shell” or “we use X for scroll” so the rule is “default + override if documented.”

---

## 4. How this doc is used

- **Guidelines today**: SKILL.md and agent-ui-structure-behavior-guidelines keep **flex-1 + min-h-0 + overflow-y-auto** and **no 100vh for content** as the default so agents have a single, predictable pattern that works in most app shells.
- **Nuance and evolution**: This doc (layout-research) records the *why*, when it applies, and what else exists. When we add alternatives or exceptions, we can update the main guidelines with short bullets (e.g. “Default: flex + min-h-0; see [layout-research.md](layout-research.md) for Grid, viewport units, and when 100vh is acceptable”) so the rule stays simple in the skill but backed by research.

So: we keep using that rule as the **fixed default** because it solves real flex/scroll/viewport issues and keeps “non-breaking UI” simple for agents; we **do** need layout research to know when it’s enough and when to recommend or allow something else. This file is the place for that research and for evolving the layout guidance over time.
