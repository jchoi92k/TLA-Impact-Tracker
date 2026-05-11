# Typography

The Learning Agency's marketing sites use **Montserrat exclusively**. We honor that for brand continuity and add **JetBrains Mono** for tabular data — alignment matters in a dashboard, and TLA's marketing sites don't have to deal with that.

---

## Font families

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

| Family | Weights | When to use |
|---|---|---|
| **Montserrat** | 400, 500, 600, 700 | Everything textual: body, headings, labels, buttons, navigation. |
| **JetBrains Mono** | 400, 500 | Tabular data: counts, dates, DOIs, IDs, citation strings, any place where character alignment matters. |

**Why not just Montserrat (matching their marketing)?** Their marketing sites ship only 400 + 700. For internal UIs we need mid-weights (500, 600) for things like active tab labels and subtle emphasis. Adding 500 and 600 is a small departure from their marketing setup but keeps us on the same family.

**Fallback stack:**
```css
font-family: 'Montserrat', system-ui, -apple-system, sans-serif;
```
```css
font-family: 'JetBrains Mono', ui-monospace, 'Courier New', monospace;
```

---

## Sizes (from [`tokens.css`](./tokens.css))

Base is **14px**. Use the scale tokens — don't invent intermediate sizes.

| Token | Pixel-ish | Real-world use |
|---|---|---|
| `--t-xs` (0.68rem) | 11px | Eyebrow labels (`IMPACT SUMMARY`), badge text |
| `--t-sm` (0.78rem) | 11px | Tab labels, meta text under a title, filter buttons |
| `--t-base` (0.88rem) | 12px | Body text inside cards, paper card titles |
| `--t-md` (1rem) | 14px | Section headings (`## Browse across datasets`) |
| `--t-lg` (1.25rem) | 18px | Card hero numbers when not using mono |
| `--t-xl` (1.55rem) | 22px | Dataset card citation counts (mono) |
| `--t-2xl` (2rem) | 28px | Dataset page title |
| `--t-hero` | 24–32px | Home headline |

---

## Weights

Prefer weight over size for emphasis. 14px-bold beats 16px-regular in most contexts and stays denser.

| Weight | Use |
|---|---|
| **400** | Body text, secondary meta |
| **500** | Author names in citations, active filter pills, subtle emphasis |
| **600** | Headings, badges, primary labels |
| **700** | Page titles, dataset card titles, brand mark, strong emphasis |

---

## Line height

| Context | Line height |
|---|---|
| Body / paragraphs | 1.55–1.7 |
| Headings (display sizes) | 1.15–1.4 |
| Buttons / labels (single line) | 1.0–1.4 |
| Citation strings (mono, multi-line) | 1.65 |

---

## Letter spacing

| Context | Tracking |
|---|---|
| Page titles (large) | `-0.02em` (tighter) |
| Headings (small) | `-0.01em` |
| Body | default (`normal`) |
| Eyebrows / uppercase labels | `0.08–0.12em` (wider — uppercase needs air) |
| Code / mono | default |

---

## Common patterns

### Section heading
```html
<h2 class="sec-head">Browse across datasets</h2>
```
```css
.sec-head {
  font-size: var(--t-md);
  font-weight: var(--w-semibold);
  letter-spacing: -0.01em;
  color: var(--ink);
}
```

### Eyebrow / small caps label
```html
<div class="eyebrow">Impact summary</div>
```
```css
.eyebrow {
  font-size: var(--t-xs);
  font-weight: var(--w-bold);
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--teal-3);
}
```

### Tabular number
```html
<div class="stat-val mono">49</div>
```
```css
.stat-val {
  font-family: 'JetBrains Mono', monospace;
  font-size: var(--t-xl);
  font-weight: var(--w-medium);
  line-height: 1;
}
```

### Citation string (multi-line mono)
```html
<div class="citation mono">Author, A. (2024). Title. <em>Journal</em>, 12(3), 45–67.</div>
```
```css
.citation {
  font-family: 'JetBrains Mono', monospace;
  font-size: var(--t-sm);
  line-height: 1.65;
  white-space: pre-wrap;
}
```

---

## Things to avoid

- **Italics inside Montserrat** unless quoting prose. Montserrat italic looks weak at UI sizes.
- **Underlines on links inside dense lists** — use color + hover-underline instead.
- **All-caps for body content** — only for short uppercase labels at `--t-xs`.
- **Mixing serif/sans** — Montserrat alone is enough for hierarchy.
- **More than 4 font sizes on one screen** — if you need more, you have a layout problem.
