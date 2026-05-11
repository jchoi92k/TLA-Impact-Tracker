# TLA Datatracker — Claude Instructions

## Data integrity rules

**Never guess or infer paper metadata.** Every field in a paper entry (title, authors, venue, published_date, year, doi, abstract, institutions, country) must come from a verified external source — OpenAlex, Semantic Scholar, or the paper itself. If a field cannot be verified via API, leave it `null`. Do not estimate, infer from context, or fill in "likely" values.

This applies to ALL fields: year, published_date, venue, venue_type, doi, abstract, authors, institutions, country. When in doubt, null.

**Only connection_strength, use_of_dataset, research_type, key_takeaway, and also_cites are allowed to be judgment calls** — those are explicitly analytical fields that require human/AI interpretation. Everything else is factual and must be sourced.

**Always tell the user the source of every field** when creating or updating paper entries. If something is null because it wasn't found in the API, say so explicitly.

## Sweep workflow

Use OpenAlex `cites:` filter as the primary source for finding papers that cite TLA dataset papers. This returns full verified metadata: doi, title, authors, venue, publication_date, abstract.

OpenAlex paper IDs:
- PERSUADE 2024: W4400046561
- PERSUADE 2023: W4385963302
- KLiCKe: W4410999734

Reconstruct abstracts from OpenAlex `abstract_inverted_index` format.

## File structure

- `data/*.json` — source of truth for all tracked papers (one file per dataset)
- `misc/pending_review.json` — new candidates awaiting user review
- `misc/review.html` — local approval UI (gitignored)
- All `misc/` content is gitignored — never commit it
