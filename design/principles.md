# Design Principles

A working set of principles for internal TLA tools. Each is cited from its source and paired with concrete implications for a dashboard-style interface.

These principles **resolve tensions** that recur in dashboard design:
- Information density vs. whitespace
- Competing elements in fixed layouts (nav, toolbars)
- When to disclose vs. when to summarize
- Color when datasets have inherent identities
- Functional tone vs. marketing polish

---

## 1. Aesthetic and Minimalist Design

> "Interfaces should not contain information that is irrelevant or rarely needed; every extra unit of information competes with the relevant units and diminishes their visibility."

**Source:** Nielsen, J. (1994, updated 2024). *10 Usability Heuristics for User Interface Design.* https://www.nngroup.com/articles/ten-usability-heuristics/

**Implication for TLA tools:** An internal lookup tool is judged on speed-to-answer, not delight. If a team member scanning citations has to skip past hero copy, gradient banners, or marketing flourishes, the tool is failing them. Strip anything that doesn't move a user toward a citation, count, or source link.

**Applied:** No animated heroes, no testimonial-style callouts, no decorative imagery. Big numbers are okay; big illustrations are not.

---

## 2. Maximize the Data-Ink Ratio

> "A large share of the ink on a graphic should present data-information; erase non-data-ink and redundant data-ink within reason."

**Source:** Tufte, E. (1983). *The Visual Display of Quantitative Information.* Graphics Press. https://www.edwardtufte.com/book/the-visual-display-of-quantitative-information/

**Implication for TLA tools:** When five datasets each have a brand color, a logo, and a venue badge, "ink" balloons fast. Prefer one count, one bar, one label per cell. Drop chrome (heavy borders, double rules, redundant legends) before dropping data.

**Applied:** Single-pixel borders. No row dividers when whitespace does the job. No "decorative" icons beside text that explains itself.

---

## 3. Hierarchy Through Weight, Not Decoration

> "Not all elements are equal — use size, color, and contrast to express importance, and de-emphasize secondary content so primary content can breathe."

**Source:** Wathan, A., & Schoger, S. (2018). *Refactoring UI.* https://www.refactoringui.com/

**Implication for TLA tools:** The nav bar tension (brand + tabs + search + status) resolves by **ranking, not balancing**. Brand is lowest-priority — users already know where they are. Current-view state and search rank highest. Status indicators de-emphasize to muted gray until they need attention.

**Applied:** Two-row header where search gets its own bar; brand mark shrinks; updated-date demotes to mono-gray. See [`components.md`](./components.md#header--navigation).

---

## 4. Progressive Disclosure

> "Defer advanced or rarely-used features to a secondary screen; show the minimum needed for the primary task, and reveal detail on demand."

**Source:** Nielsen, J. (2006). *Progressive Disclosure.* Nielsen Norman Group. https://www.nngroup.com/articles/progressive-disclosure/

**Implication for TLA tools:** A citation row should show title, venue, year, and connection strength. Authors (full list), abstract, DOI, institutions, also-cites belong in an expand-to-detail panel. This keeps scan density high without sacrificing the verified-source rigor the data integrity rules demand.

**Applied:** The "Show details" button on paper cards reveals the full citation, institutions, countries, also-cites, and abstract. Scan stays fast; depth stays available.

---

## 5. Recognition Rather Than Recall

> "Minimize memory load by making objects, actions, and options visible; the user should not have to remember information from one part of the interface to another."

**Source:** Nielsen, J. (1994, updated 2024). *10 Usability Heuristics.* https://www.nngroup.com/articles/ten-usability-heuristics/

**Implication for TLA tools:** Dataset names (PERSUADE, KLiCKe) and field provenance (OpenAlex vs. GS vs. manual) should be labeled in-line wherever they appear. Don't make team members remember which dataset uses which color, or recall that a missing field means "not verified."

**Applied:** Cross-dataset views show the dataset name as a tinted badge on every row. Source-of-data hints (`abstract_source: openalex`) appear on demand inside the detail panel.

---

## 6. Color Used Functionally, Not Decoratively

> "Use color to encode meaning, not to brand surfaces; reserve saturated color for what needs attention and let neutrals carry the layout."

**Source:** IBM Carbon Design System. *Color usage.* https://carbondesignsystem.com/elements/color/overview/ — combined with Refactoring UI's color guidance: https://www.refactoringui.com/

**Implication for TLA tools:** Datasets have inherent color identities, but if every row is tinted by its dataset, color stops meaning anything. Use neutrals for the table body, restrict dataset color to a single 3-pixel rule or chip, and reserve red/amber/teal for actual states (failed lookups, stale data, new entries).

**Applied:** Dataset color appears as: (a) a 3px left rule on dataset cards, (b) the dataset card's count number, (c) a tinted badge in cross-dataset views. Never as a row tint, never as the page background.

---

## 7. Opinionated Defaults Over Configurable Flexibility

> "A purpose-built tool should make the right choice for the user, not surface every option; flexibility that lets everyone invent workflows creates chaos at scale."

**Source:** Linear. *The Linear Method — Principles & Practices.* https://linear.app/method/introduction

**Implication for TLA tools:** Resist building filter panels, view toggles, and customization for an org of a handful of users. Ship one default sort (most recent), one default view (list), one default grouping (by dataset). Add toggles only when a real second use-case appears.

**Applied:** Filters are pills, not dropdowns. No "sort by" menu — recency wins. No "view as table / cards / grid" toggles. No saved searches. If a feature is built, it's because someone asked for it twice.

---

## 8. Functional, Not Marketing

> "Foundations (color, type, layout, motion) should serve task completion across contexts; design tokens systematize decisions so the surface stays calm and consistent."

**Source:** Google. *Material Design 3 — Foundations.* https://m3.material.io/foundations

**Implication for TLA tools:** Internal tools earn trust by feeling like instruments. No hero sections, no testimonial-style callouts, no animated gradients. Dense type, generous line-height, neutral surfaces, and motion only to communicate state (loading, sort applied, row expanded) — patterns Stripe, Linear, and Vercel use precisely because their users are working, not browsing.

**Applied:** The home hero is informational, not promotional. Stats are the largest type on the page, not headlines. Motion is reserved for state confirmation (a copy button briefly turning teal).

**No emojis in public UI.** Pictograms like 📊 📄 🔗 ⚑ look casual and inconsistent across platforms (they render differently on Windows/macOS/Linux/mobile). Use text labels and typographic Unicode arrows (`↗`, `→`, `←`) instead. Internal-only tooling (`misc/tracker.html`) is free to use emojis where they improve editor affordances (e.g. `⚠` next to a missing-abstract banner). The boundary is "is this rendered to org-external visitors": if yes, no emojis.

---

## Applying the recurring tensions

### Density vs. whitespace
Start dense (Tufte, Carbon). Add whitespace **around groups, not within rows**. Refactoring UI's "start with too much white space then refine" applies at the *section* level — between hero and grid, between grid and table — not within individual rows.

### Competing nav elements
Rank by frequency-of-use, not by stakeholder importance. Brand goes smallest-left; search and current-view-state get prominence; status pills demote to neutral until they fire.

### Color restraint with dataset identities
Confine each dataset's color to one identifying chip per row. Body stays neutral.

### Functional, not marketing
Borrow Linear's restraint. Every pixel earns its place by helping someone find a citation faster.

---

## Sources

- [Nielsen, J. — 10 Usability Heuristics for User Interface Design](https://www.nngroup.com/articles/ten-usability-heuristics/)
- [Nielsen, J. — Progressive Disclosure](https://www.nngroup.com/articles/progressive-disclosure/)
- [Tufte, E. — The Visual Display of Quantitative Information](https://www.edwardtufte.com/book/the-visual-display-of-quantitative-information/)
- [Wathan, A. & Schoger, S. — Refactoring UI](https://www.refactoringui.com/)
- [IBM Carbon Design System — Color](https://carbondesignsystem.com/elements/color/overview/)
- [Linear — The Linear Method](https://linear.app/method/introduction)
- [Material Design 3 — Foundations](https://m3.material.io/foundations)
