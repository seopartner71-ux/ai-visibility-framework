# Dataset Schema v1.1

**Status:** v1.1 — Operational Methodology / Pilot
**Note:** This file describes the schema. Actual data lives separately, e.g. `research/data/pilot-v1.1.csv`. Do not name the raw data file `dataset.md` — it is not a dataset, it is this schema description.

---

## 1. Identifier fields

| Field | Description |
|---|---|
| `project_id` | Identifies the engagement/client project. |
| `brand_id` | Identifies the target brand within a project. |
| `query_set_id` | Identifies the frozen query set used (e.g. `QSET-001`). Required so query sets can be versioned and compared year over year. |
| `run_id` | Identifies a single measurement run (one full pass across the query set). |
| `measurement_run_id` | Composite key for a specific run, recommended format: `{project_id}_{brand_id}_{query_set_id}_{run_id}`. |
| `engine` | AI engine evaluated (e.g. ChatGPT, Gemini, Yandex Neuro, Perplexity). |
| `model` | Specific model/version string of the engine, where available. |
| `query_id` | Identifier of the individual query within the query set. |
| `query_category` | Category label: Brand / Category / Comparison / Recommendation / Informational, etc. |

## 2. Validity fields

| Field | Description |
|---|---|
| `response_valid` | 1 if the engine returned a usable response; 0 if error/timeout/refusal. Invalid responses are excluded from all denominators, not counted as "brand absent." |
| `error_flag` | Free-text or coded reason when `response_valid = 0`. |

## 3. Mention / citation fields

| Field | Description |
|---|---|
| `brand_mentioned` | 1/0 — explicit brand mention present. |
| `brand_mention_count` | Integer count of brand mentions within the response. |
| `competitor_mention_count` | Integer count of tracked competitor mentions within the response (used as the SOV denominator base). |
| `citation_present` | 1/0 — at least one qualifying citation present (see `metric-definitions-v1.1.md` §2). |
| `citation_count` | Integer count of qualifying citations. |
| `citation_sources` | List field — every qualifying source type present (e.g. official site, LinkedIn, Forbes). Not a single-value field — a response can cite multiple source types. |
| `citation_urls` | List field — actual URLs cited, where extractable. |

## 4. Position / prominence fields

| Field | Description |
|---|---|
| `brand_position` | Rank/order of the brand mention among all entities mentioned in the response. |
| `total_entities_mentioned` | Total count of distinct entities mentioned in the response (used in the Prominence formula). |
| `prominence_score` | Precomputed 0–100 value per `metric-definitions-v1.1.md` §5 (can be derived at analysis time instead of stored, but the formula must match exactly). |

## 5. Recommendation / framing fields

| Field | Description |
|---|---|
| `recommendation` | 1/0 — brand was recommended (only applicable to query_category = Recommendation). |
| `recommendation_position` | Rank of the brand among recommended entities, where applicable. |
| `framing_primary` | Primary framing classification: Leader / Recommended / Neutral / Alternative / Warning / Negative. |
| `framing_secondary` | Optional secondary framing note, for edge cases that don't cleanly fit one category. |

---

## 6. Query construction rules

Frozen before any measurement begins:

- Queries are **predefined** and **version-controlled** under a `query_set_id`.
- Queries are **not modified** after results have been seen, for any run using that `query_set_id`. A change requires a new `query_set_id`.
- Queries are **market-specific** and **language-specific** — a query set built for the Russian market/language is not reused as-is for another market.
- **No query may contain the target brand name unless the query category explicitly requires it** (i.e. `Brand` category queries only). Including the brand name in Category, Comparison, or Recommendation queries artificially inflates Mention Rate and invalidates the measurement.

---

## 7. Why these fields matter

The original v1.0 draft dataset (`brand_mentioned, citation_present, brand_position, recommendation, framing`) cannot reproduce the metrics defined in `metric-definitions-v1.1.md` — there is no way to compute Mention Share, Prominence, or a non-error-conflated Mention Rate from those five fields alone. The fields above are the minimum required set for every metric in the scoring rubric to be independently recomputed from raw data.
