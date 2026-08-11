# Query Set Methodology v1.1

**Status:** v1.1 — Operational Methodology / Pilot
**Related:** `dataset-schema.md` (query_set_id field), `metric-definitions-v1.1.md` (query-level vs response-level metrics)

## Purpose

The query set is the instrument through which AI Visibility is measured. If the instrument is inconsistent — reworded between runs, contaminated with the brand name where it shouldn't be, or mixed across markets — every downstream metric inherits that inconsistency, no matter how correct the scoring formulas are. This document defines how a query set is built, frozen, and reused so that measurements taken months apart remain comparable.

---

## 1. Query Categories

Every query in a set belongs to exactly one category. Category determines both what the query may contain and which metrics it feeds.

| Category | Purpose | May contain brand name? |
|---|---|---|
| Brand | Measures how the AI represents the brand when asked directly | Yes — required |
| Category | Measures organic presence when the brand is not named | No |
| Comparison | Measures standing against named or implied competitors | Only if comparing to a competitor by name, not the target brand |
| Recommendation | Measures whether the brand is suggested unprompted | No |
| Informational | Measures topical authority independent of any brand | No |

Each category maps to specific metrics in `metric-definitions-v1.1.md` — e.g. only Recommendation-category queries populate the Recommendation Rate denominator. Mixing categories in analysis (e.g. computing Recommendation Rate over all queries instead of the Recommendation subset) invalidates the metric.

---

## 2. Construction Rules

These rules are fixed before any measurement begins and apply for the lifetime of a given `query_set_id`:

- **Predefined.** The full list of queries is written and reviewed before the first run, not assembled ad hoc during measurement.
- **Version-controlled.** Every query set is assigned an ID (e.g. `QSET-001`) and stored under that ID permanently.
- **Frozen after first use.** Once a query set has been used in a run, its wording cannot be edited. A wording change — even a minor one — produces a new `query_set_id` (e.g. `QSET-002`), not a silent revision of `QSET-001`. This is what allows two runs months apart to be compared honestly.
- **Not modified after seeing results.** Queries are never adjusted based on which ones "worked better" for the brand. This is the single most common source of bias in AI visibility measurement and is treated as a hard rule, not a guideline.
- **Market-specific.** A query set built for one market is not reused as-is in another — purchasing intent, terminology, and competitor sets differ by market.
- **Language-specific.** Translation is not equivalent to a new query set — phrasing that triggers a citation in one language may not in another. Each language gets its own constructed (not translated) set.

---

## 3. The No-Brand-Name Rule

**No query may contain the target brand name unless its category explicitly requires it** — in practice, this means only `Brand`-category queries may name the brand.

Rationale: including the brand name in a Category, Comparison, or Recommendation query tells the AI system what answer is expected, which artificially inflates Mention Rate and Recommendation Rate and produces a number that reflects the query design, not the brand's actual visibility. This is the query-set equivalent of a leading question in a survey — the result is not usable, however clean the rest of the methodology is.

**Verification step:** before a query set is frozen, every query is checked against this rule as a final review pass — flagged violations are rewritten before the `query_set_id` is locked, not after.

---

## 4. Set Size and Composition

- A pilot query set (v1.1 default) is **60 queries**, distributed across the five categories above. Distribution is documented per project — e.g. 10 Brand / 15 Category / 10 Comparison / 15 Recommendation / 10 Informational — and that distribution itself is versioned along with the query set.
- Set size may scale with project maturity, but any change in size or category distribution requires a new `query_set_id`.

---

## 5. Multi-Engine Application

The same frozen query set is run identically across all evaluated engines (ChatGPT, Gemini, Yandex Neuro, Perplexity, etc.) within a single `run_id`. Engine-specific rewording is not permitted — if an engine requires different phrasing to produce a comparable response type, that is itself a finding to document, not a reason to alter the query.

---

## 6. What This Enables

With these rules in place, `metric-definitions-v1.1.md` and `scoring-rubric-v1.1.md` become reproducible in the strict sense: a second auditor working from the same `query_set_id` and the same dataset schema will run the same 60 queries, in the same categories, with no brand-name contamination, and arrive at the same raw metrics before any scoring formula is even applied. Query set integrity is the foundation the rest of the measurement stack depends on — errors here cannot be corrected downstream by better scoring math.
