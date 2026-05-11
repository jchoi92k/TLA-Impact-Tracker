# The Learning Agency — Design System

A working design system for internal TLA tools. Built around the brand identity at [the-learning-agency.com](https://the-learning-agency.com) and [the-learning-agency-lab.com](https://the-learning-agency-lab.com), informed by reputable UX/UI guidance for data-dense internal interfaces.

This package is **portable** — copy this `/design/` folder (or extract to its own repo) to bootstrap any new TLA-themed app with consistent visual language.

---

## What's in here

| File | What it gives you |
|---|---|
| [`tokens.css`](./tokens.css) | Drop-in CSS variables. Paste into your `<style>` block and start using. |
| [`tokens.md`](./tokens.md) | Token reference with rationale and usage rules. |
| [`typography.md`](./typography.md) | Font stack, sizes, weights, line heights — what to use where. |
| [`components.md`](./components.md) | Reference patterns for common components (cards, badges, pills, buttons, paper records). |
| [`principles.md`](./principles.md) | UX/UI principles guiding design decisions. Cite-able sources, applicable rules. |

---

## Quick start (5 minutes)

1. **Copy `tokens.css` into your `<style>` block** (or include it as a stylesheet).
2. **Add the Google Fonts link** in your `<head>`:
   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com">
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
   <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
   ```
3. **Set the base on `body`**:
   ```css
   body {
     font-family: 'Montserrat', system-ui, sans-serif;
     background: var(--bg);
     color: var(--ink);
     font-size: 14px;
     line-height: 1.55;
     -webkit-font-smoothing: antialiased;
   }
   ```
4. **Read [`principles.md`](./principles.md) before designing screens.** Internal tools need different choices than marketing pages, and the principles document explains the tensions to resolve (density vs. whitespace, color restraint, progressive disclosure).

---

## When NOT to use this system

This package is for **internal-facing** TLA tools. If you're building:

- Public marketing for TLA → use the live WordPress/Elementor site as reference instead. It's more decorative.
- A non-TLA project → don't drag the teal everywhere; it's a brand mark.
- A heavily branded co-promotion (Lab + partner) → check with the partner's identity first.

---

## How this system was built

- **Brand tokens** were extracted from live CSS at the-learning-agency.com and the-learning-agency-lab.com (see [`tokens.md`](./tokens.md) for the audit).
- **Principles** synthesize Nielsen Norman Group heuristics, Tufte's data-ink ratio, Refactoring UI's hierarchy guidance, IBM Carbon's color rules, and observable patterns from Linear / Vercel / Stripe dashboards. All sources cited inline in [`principles.md`](./principles.md).
- **Components** are working patterns from `index.html` in the TLA Impact Tracker — proven in production rather than aspirational.

---

## How to evolve this system

When a new tool diverges from these tokens or principles intentionally:

1. **Document the divergence in your tool's own README**, with reasoning. Don't silently fork.
2. **If the divergence is generally useful, propose it back here** by editing the relevant file.
3. **Don't add tokens without a need** — every new token is a new decision someone must make. Reuse aggressively.

Last updated: 2026-05
