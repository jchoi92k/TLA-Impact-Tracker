# TLA Datatracker — Claude Instructions

## Data integrity rules

**Never guess or infer paper metadata.** Every field in a paper entry (title, authors, venue, published_date, year, doi, abstract, institutions, country, full_citation) must come from a verified external source — OpenAlex, GS cite-button, arXiv API, or the paper itself. If a field cannot be verified, leave it `null`. Do not estimate, infer from context, or fill in "likely" values.

**Only `connection_strength`, `research_type`, `key_takeaway`, and `also_cites` are allowed to be judgment calls** — these are explicitly analytical fields that require interpretation. `ai_suggestion` is also a judgment field (AI-generated, user-reviewable). The `use_of_dataset` field was removed in 2026-05 as too speculative when generated from abstract alone.

### `connection_strength` rubric (AI-judged from abstract)

Used by Claude during the AI-judging step. Document anywhere it changes.

- **strong** — paper directly uses the dataset for training, evaluation, or experiments. Abstract usually names it explicitly ("We evaluate on PERSUADE 2.0…", "trained on the Feedback Prize – Predicting Effective Arguments dataset").
- **moderate** — paper references the dataset for comparison/benchmarking, or builds on closely related methods. Sister-resource papers (a parallel corpus in another language) also fit here.
- **weak** — paper mentions the dataset only peripherally — survey, related-work list, background. The abstract often doesn't name the dataset at all; "weak" means "GS lists this in cited-by, but the abstract gives no further signal."

**Known limits to be honest about:**
- Claude only sees title + abstract + dataset description. It does not read the full paper.
- For papers where the abstract doesn't mention the dataset by name, the bucket is a *hedged inference* from the paper's topic and the fact that GS placed it in the cited-by list.
- Borderline moderate↔weak cases are genuinely uncertain; the user reviews these in the tracker before promoting.

**Tell the user the source of every field** when creating or updating paper entries. If something is null because it wasn't found, say so explicitly.

## Monthly sweep workflow

The pipeline runs monthly, ~5 minutes of user time per sweep. Run in this order:

```bash
# 1. Sweep Google Scholar — finds new citations, skips already-approved/rejected papers
python misc/sweep_gs.py                  # all datasets
python misc/sweep_gs.py --dataset persuade  # one dataset only

# 2. Ask Claude Code (in this session, or `claude` in your terminal) to pre-fill
#    judgment fields. Claude reads the pending JSONs, reasons over each paper's
#    abstract + dataset context, and writes back:
#      - connection_strength  (strong/moderate/weak)
#      - research_type        (short label)
#      - key_takeaway         (1-2 interpretive sentences)
#      - ai_suggestion        (approve/review/reject — sets status accordingly)
#    Skip also_cites (too uncertain without references list — leave for human).
#    Covered by your Claude subscription — NO Anthropic API key, no per-token cost.

# 3. Open the local tracker UI to review + adjust + promote
python misc/serve.py                     # auto-opens http://localhost:8765/tracker.html

# 4. In the tracker: tweak any AI judgments you disagree with, batch-promote
#    Approved papers move to data/*.json; rejected go to rejected.json (full paper preserved)

# 5. Push to publish — GitHub Pages auto-updates
git add data/
git commit -m "Monthly sweep YYYY-MM"
git push
```

## Pipeline guarantees

- **Atomic writes** everywhere — crashes never corrupt JSON.
- **Idempotent re-runs** — sweep preserves prior enrichment + judgment fields (does not destroy work-in-progress). See `merge_with_prior()` in `sweep_gs.py`.
- **No duplicates** — sweep skips titles already in `data/*.json` (approved) AND in `rejected.json`. Both checks are title-normalized (lowercase, whitespace-collapsed).
- **Auto-backups** before `approve_pending.py` modifies `data/*.json` (keeps last 5).
- **One API trip per paper, max** — OpenAlex enrichment packs everything (abstract, doi, authorships, institutions, countries) into one `select=` query. Never chain follow-ups.
- **Throttling** — GS interactions ≥2.5s apart; arXiv ≥3s; OpenAlex ≥1s. Per-API throttle tracker prevents back-to-back hits.

## Pipeline file roles

| File | Role |
|---|---|
| `misc/sweep_gs.py` | GS sweep + arXiv/OpenAlex/page enrichment + cite-button extraction |
| `misc/serve.py` | Local HTTP server (`:8765`) backing the tracker UI |
| `misc/tracker.html` | Review UI — Pending / Rejected / History tabs, inline edits, promote modal |
| `misc/approve_pending.py` | Moves approved → `data/*.json`, rejected → `rejected.json`, logs to `history.json` |
| `misc/pending/{ds}.json` | Per-dataset queue, never overwrites other datasets |
| `misc/rejected.json` | Blocklist (full paper preserved for lossless restore) |
| `misc/history.json` | Append-only audit log of all approve/reject/restore events |
| `misc/sweep_errors.log` | Crash log with remediation hints |
| `data/*.json` | Source of truth — committed to git, served by GitHub Pages |
| `data/*.json.bak.*` | Auto-rotated backups (last 5 per dataset) — gitignored |

## OpenAlex source paper IDs (for cite-filter sweeps if needed)

- PERSUADE 2024: W4400046561
- PERSUADE 2023: W4385963302
- KLiCKe:        W4410999734

## Design system

Brand tokens, typography, components, and UX principles live in `/design/`. Start with `design/README.md`. Any new TLA-themed app should reuse this package.

## What stays gitignored

All `misc/` content is gitignored. Never commit:
- `misc/pending/`, `misc/rejected.json`, `misc/history.json` (curation state)
- `misc/sweep_errors.log`, `data/*.json.bak.*` (logs/backups)
- `misc/*.html` (local-only tools)

Only `data/*.json`, `index.html`, `design/`, `CLAUDE.md`, and `README.md` are public-facing.
