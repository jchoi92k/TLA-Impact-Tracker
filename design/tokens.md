# Design Tokens — Reference

CSS variables defined in [`tokens.css`](./tokens.css). This document explains **why** each token exists and **when** to use it.

---

## Color

### Brand

| Token | Hex | Used for | Source |
|---|---|---|---|
| `--teal` | `#1ea69a` | Primary accent: links, focus rings, active nav, primary buttons. Used **sparingly** as the single load-bearing color. | the-learning-agency.com (appears 27× in CSS — clearly the brand color) |
| `--teal-2` | `#40b797` | Lighter teal for hover states or secondary fills. | the-learning-agency.com |
| `--teal-3` | `#00897e` | Darker teal for emphasized text on tinted backgrounds. | derived |
| `--teal-bg` | `#e9f6f4` | Tinted surface for callouts (impact summary cards, info banners). | derived |
| `--orange` | `#F7943B` | Lab brand. **Reserve for one accent role per surface** — typically "new" pills or Lab-specific tags. Do not mix with teal as a co-equal accent. | the-learning-agency-lab.com (Elementor `--e-global-color-primary`) |
| `--blue` | `#6196ff` | Their tertiary accent. Use sparingly for secondary links or selected-row states. | both sites (tag/hover) |

**Rule:** A page should have one dominant accent color. Teal is the default. If you need a second accent (warning, "new"), use orange — never another teal shade.

### Neutrals

| Token | Hex | Used for |
|---|---|---|
| `--bg` | `#F8FAFC` | Page background. Subtle cool tint, not pure white. |
| `--surface` | `#FFFFFF` | Cards, modals, anything raised. |
| `--ink` | `#2A2A2A` | Body text. Near-black, never pure black (softer on the eye). |
| `--slate` | `#54595F` | Secondary text (metadata, captions, descriptions). |
| `--slate-2` | `#6B7280` | Tertiary text (timestamps, footers, less important meta). |
| `--slate-3` | `#9CA3AF` | Placeholders, disabled states. |
| `--border` | `#E5E5E5` | Component borders, dividers. |
| `--border-2` | `#EFEFEF` | Subtle internal dividers (within a card). |

**Rule:** Default to neutrals. Color is for meaning, not decoration. The first instinct when adding visual interest should be: "can I do this with weight, size, or spacing instead?"

### Functional status

| Token | Hex | Hex (bg) | Meaning |
|---|---|---|---|
| `--strong` | `#047857` | `--strong-bg: #ECFDF5` | Success, strong engagement, direct use |
| `--moderate` | `#B45309` | `--moderate-bg: #FFFBEB` | Attention, moderate engagement |
| `--weak` | `#B91C1C` | `--weak-bg: #FEF2F2` | Warning, weak engagement |
| `--flagged` | `#7C3AED` | `--flagged-bg: #F5F3FF` | Needs review, anomalous |

**Rule:** These colors carry semantic weight. Don't reuse them for unrelated UI states.

### Per-dataset (TLA Impact Tracker)

| Token | Hex | Dataset |
|---|---|---|
| `--ds-persuade` | `#F59E0B` | PERSUADE 2.0 |
| `--ds-piilo` | `#60A5FA` | PIILO |
| `--ds-klicke` | `#34D399` | KLiCKe |
| `--ds-asap2` | `#A78BFA` | ASAP 2.0 |

**Rule:** Confine dataset color to one identifying chip or a 3-pixel rule per surface. **Do not tint backgrounds, fills, or large surfaces with dataset colors** — at the page level, dataset identity is contextual (you're already on its page), not a visual theme.

---

## Type scale

14px base. Minor-third (≈1.2) scale rounded for legibility:

| Token | Size | Use |
|---|---|---|
| `--t-xs` | 0.68rem (~11px) | Uppercase eyebrow labels, badges |
| `--t-sm` | 0.78rem (~11px) | Meta text, captions, tab labels |
| `--t-base` | 0.88rem (~12px) | Body content in cards, table cells |
| `--t-md` | 1rem (14px) | Section headings |
| `--t-lg` | 1.25rem (~18px) | Page subtitles |
| `--t-xl` | 1.55rem (~22px) | Card numbers, metrics |
| `--t-2xl` | 2rem (~28px) | Page titles |
| `--t-hero` | clamp(1.7rem, 3.5vw, 2.3rem) | Hero / landing headlines |

See [`typography.md`](./typography.md) for full font-stack and weight guidance.

---

## Weight

Prefer **weight changes over size changes** for emphasis. Five sizes is plenty for any screen.

| Token | Weight | Use |
|---|---|---|
| `--w-regular` | 400 | Body |
| `--w-medium` | 500 | Quiet emphasis (author names, active tab) |
| `--w-semibold` | 600 | Headings, primary labels |
| `--w-bold` | 700 | Page titles, brand, strong emphasis |

---

## Spacing

Use multiples. Do not invent new values.

| Token | Value | Use |
|---|---|---|
| `--sp-1` | 4px | Tight gaps (icon-text) |
| `--sp-2` | 8px | Small gaps |
| `--sp-3` | 12px | Default field padding |
| `--sp-4` | 16px | Card padding bottom |
| `--sp-5` | 20px | Card padding |
| `--sp-6` | 24px | Section gaps |
| `--sp-8` | 32px | Major section gaps |
| `--sp-10` | 40px | Hero padding |
| `--sp-12` | 48px | Outer page rhythm |

---

## Radius

| Token | Value | Use |
|---|---|---|
| `--r-sm` | 5px | Small buttons, tags |
| `--r-md` | 7px | Cards, inputs |
| `--r-lg` | 10px | Modals, hero callouts |
| `--r-pill` | 20px | Pills, badges |

---

## Shadow

Use sparingly. Internal tools rarely need raised surfaces.

| Token | Use |
|---|---|
| `--shadow-1` | Card hover (subtle) |
| `--shadow-2` | Dataset card hover |
| `--shadow-3` | Modals, overlays |

---

## Motion

| Token | Value | Use |
|---|---|---|
| `--motion` | 0.12s | Default for hover/focus |
| `--motion-2` | 0.2s | Layout shifts |
| `--ease` | `cubic-bezier(.4, 0, .2, 1)` | Standard easing |

**Rule:** Animate state changes (a row expanding, a button confirming a copy). Don't animate for decoration. No fade-ins on load. No parallax. No marquees.
