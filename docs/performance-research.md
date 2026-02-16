# Frontend Performance: Why LCP/INP/CLS and When to Optimize

The baro7 [application-development-guidelines.md](application-development-guidelines.md) §9 mentions LCP, INP, CLS, lazy loading, and bundles. This doc explains why those metrics and strategies matter, when they apply, and what else to consider so we can evolve performance guidance without turning it into a full performance manual.

---

## 1. Why these rules exist

- **Core Web Vitals**: LCP (Largest Contentful Paint), INP (Interaction to Next Paint), and CLS (Cumulative Layout Shift) are user-centric signals: “Did the main content show quickly?” “Did clicks feel responsive?” “Did the page jump?” They affect both UX and search/surface ranking where CWV are used.
- **Lazy loading**: Below-the-fold or heavy content (images, heavy components) do not need to block first paint. Deferring them reduces initial payload and improves LCP and INP.
- **Bundles and code-splitting**: One huge JS bundle delays interactivity. Splitting by route or feature (dynamic import) keeps initial load smaller so the app becomes responsive sooner.

So the rules are not arbitrary: they target measurable user impact (paint, interaction, stability) and common levers (load less upfront, split code).

---

## 2. When they apply

- **LCP**: Any page with a dominant image or block of text. Optimize above-the-fold images (size, format, priority), server or edge rendering for critical HTML, and avoid blocking scripts so the largest element can paint quickly.
- **INP**: Any page with clicks or key input. Reduce main-thread work (smaller JS, avoid long tasks), use non-blocking patterns for handlers. Especially important for custom UI and heavy client-side logic.
- **CLS**: Any layout that can shift (images without dimensions, late-loaded content, dynamic inserts). Reserve space (width/height or aspect-ratio), avoid inserting content above existing content without reserving space, and prefer transform/opacity for animations where possible.
- **Lazy load**: Use for images below the fold, modals, tabs that are not initial, or heavy components (charts, editors). Do not lazy load the primary content; that hurts LCP.
- **Code-split**: Use for routes or features that are not needed on first load. Framework routers (e.g. Next.js, React Router) often support this via dynamic import; keep the entry and critical path small.

---

## 3. Alternatives and references

- **Metrics**: [web.dev Core Web Vitals](https://web.dev/vitals/). INP replaces FID; LCP and CLS definitions are standard. Use Chrome DevTools, PageSpeed Insights, or RUM to measure.
- **Images**: Modern formats (WebP, AVIF), responsive images (`srcset`, `sizes`), and explicit dimensions or aspect-ratio reduce CLS and improve LCP.
- **Frameworks**: Next.js image component, Remix, and others handle a lot of this; follow project patterns. If the project has a performance budget, stay within it.

---

## 4. How this doc is used

- **Guidelines today**: application-development-guidelines §9 keeps the checklist short: consider LCP/INP/CLS, lazy load where appropriate, follow project caching and asset patterns. Agents get a fixed set of rules.
- **Nuance and evolution**: This doc (performance-research) records the *why*, when each lever applies, and references. When we add more specific bullets (e.g. “reserve space for above-the-fold images”), we can update §9 briefly and keep the detail here.

So: we keep §9 as the default performance checklist; we use this file to back it with rationale and to evolve frontend performance guidance over time.
