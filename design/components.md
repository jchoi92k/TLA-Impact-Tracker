# Component Patterns

Reusable patterns from the TLA Impact Tracker. These are reference implementations — copy, adapt, document deviations.

For tokens see [`tokens.css`](./tokens.css) and [`tokens.md`](./tokens.md). For principles see [`principles.md`](./principles.md).

---

## Header / Navigation

**Pattern: two-bar header.** Primary nav stays minimal; persistent search occupies its own bar. Resolves the "everything competing for top-right" tension flagged in `principles.md` §3 (Hierarchy Through Weight, Not Decoration).

```
┌──────────────────────────────────────────────────────────────┐
│ TLA Impact   PERSUADE  PIILO  KLiCKe  ASAP  Jo  Updated YYYY │   nav bar (compact)
├──────────────────────────────────────────────────────────────┤
│ 🔍 Search across all papers, authors, institutions…          │   search bar (always visible)
└──────────────────────────────────────────────────────────────┘
```

- Brand: `--w-bold`, `--t-sm`, just `TLA · <accent>Impact</accent>`.
- Tabs: `--t-sm`, `--w-medium`, `--r-md` rounded background on active.
- Updated indicator: `--t-xs`, `--slate-3`, mono. Hidden below 720px.
- Search row: full-width input, generous padding, leading icon. Default state is calm, focus state is teal-bordered.

---

## Dataset card (home grid)

Identifies a dataset and previews its count. Left edge is the only place dataset color appears.

```html
<a href="#/dataset/persuade" class="ds-card" style="--c:#F59E0B">
  <div class="ds-card-name">PERSUADE 2.0</div>
  <div class="ds-card-year">Published 2024</div>
  <div class="ds-card-count">49</div>
  <div class="ds-card-lbl">Citations tracked</div>
</a>
```

Key choices:
- 3-pixel left border in dataset color. Body stays neutral.
- Citation count in `JetBrains Mono`, large, dataset color.
- Hover: 1px translate + light shadow. No background change.

---

## Pill / filter button

```html
<button class="pill on">All</button>
<button class="pill">Strong</button>
```

- Off state: white surface, neutral border, slate text.
- Hover: teal border, teal text.
- On state: teal fill, white text.
- Always `--r-pill`. Same height across a row.

---

## Badge

Pill's smaller cousin. For state, not action.

```html
<span class="badge b-strong">Strong</span>
<span class="badge b-moderate">Moderate</span>
<span class="badge b-new">NEW</span>
```

| Class | Background | Text | When |
|---|---|---|---|
| `b-strong` | `--strong-bg` | `--strong` | Strong engagement |
| `b-moderate` | `--moderate-bg` | `--moderate` | Moderate engagement |
| `b-weak` | `--weak-bg` | `--weak` | Weak engagement |
| `b-flagged` | `--flagged-bg` | `--flagged` | Needs review |
| `b-new` | `--orange` (solid) | white | New this sweep |
| `b-ds` | dataset color @ 20% | dataset color | Dataset identifier in cross-context views |

Rule: at most 3 badges per card head. If you need more, you have a layout problem.

---

## Paper card

The dominant unit in the tracker. Designed for scan first, drill on demand.

```html
<div class="paper">
  <div class="paper-head">
    <div class="paper-title">
      <a href="https://doi.org/...">Title of the paper</a>
    </div>
    <div class="paper-badges">
      <span class="badge b-ds">PERSUADE 2.0</span>
      <span class="badge b-new">NEW</span>
      <span class="badge b-strong">Strong</span>
    </div>
  </div>
  <div class="paper-meta">
    <span class="authors">Author, A. et al.</span> · Venue, 2025
  </div>
  <div class="paper-use">One sentence on how this paper uses the dataset.</div>
  <div class="paper-takeaway">Italic key takeaway, more interpretive.</div>
  <div class="paper-actions">
    <button class="btn-action">Show details</button>
    <button class="btn-action">Copy citation</button>
  </div>
  <div class="paper-detail" style="display:none">
    <!-- progressive-disclosure detail panel -->
  </div>
</div>
```

Layering follows `principles.md` §4 (Progressive Disclosure):
1. **Always visible:** title, badges, author/venue/year, use-of-dataset, key-takeaway.
2. **On expand:** full citation, full venue, institutions, countries, also-cites, abstract, links.

---

## Detail list (key/value)

Used inside expanded paper detail.

```html
<dl class="detail-list">
  <dt>Citation</dt> <dd><div class="citation mono">…</div></dd>
  <dt>Institutions</dt> <dd>Cornell · MIT · Stanford</dd>
  <dt>Countries</dt> <dd>USA, UK, Germany</dd>
  <dt>Also cites</dt> <dd><a href="#/dataset/piilo">PIILO</a></dd>
  <dt>Links</dt> <dd><a href="…">DOI ↗</a> · <a href="…">Open paper ↗</a></dd>
  <dt>Abstract</dt> <dd><div class="abstract">…</div></dd>
</dl>
```

- Grid: `minmax(110px, max-content) 1fr`. Labels right-edge align, values flow.
- Labels: `--t-xs`, `--w-bold`, uppercase, `--slate-2`.
- Values: `--t-base`, normal weight, `--ink`.
- Citation values: mono.
- Stacks to 1 column below 780px.

---

## Action buttons (in-card)

For inline actions like "Show details" or "Copy citation."

```html
<button class="btn-action">Show details</button>
<button class="btn-action copied">Copied</button>
```

- Default: white surface, neutral border, slate text.
- Hover: teal border, teal text.
- Confirmed state (after copy): teal fill, white text, auto-revert after 1.4s.

---

## Impact summary card (narrative blurb)

A teal-tinted callout for auto-generated narrative text. Slide-ready copy.

```html
<div class="narrative">
  <div class="narrative-lbl">Impact summary</div>
  <div class="narrative-text">PERSUADE 2.0 has been cited 49 times across 2022–2026. 28 papers use the dataset directly; 14 reference it for benchmarking; 7 are incidental mentions. Research originates from 8 countries.</div>
  <button class="btn-action">Copy summary</button>
</div>
```

- Subtle teal-bg fill. Single accent on the page.
- Max-width ~820px (prose readability).
- Bottom button is the only action.

---

## Stat / metric

```html
<div>
  <div class="stat-val mono">49</div>
  <div class="stat-lbl">Citations tracked</div>
</div>
```

- Number: mono, medium weight, large.
- Label: uppercase, `--t-xs`, `--w-semibold`, `--slate-2`.

Group with `display: flex; gap: var(--sp-10)`.

---

## States to handle

For every list-based component, design for all four:

1. **Loading**: spinner + "Loading …" text, centered.
2. **Empty**: helpful message, no fake content.
3. **Error**: message + remediation hint.
4. **Populated**: the design above.

The `.loading` and `.empty` utility classes are defined in `tokens.css`. Use them.
