# Metric Definitions v1.1

**Status:** v1.1 — Operational Methodology / Pilot
**Part of:** AI Visibility Framework
**Purpose:** This document defines every metric used in the AI Visibility Index (AVI) and AI Readiness Index (ARI) with explicit numerator, denominator, unit, and measurement level, so that two independent auditors working from the same dataset produce the same result.

---

## 0. Core Principles

- Every metric is defined at either **response-level** or **query-level**. These use different denominators and must not be mixed.
- A response only enters any denominator if `response_valid = 1` (see `dataset-schema.md`). Engine errors, timeouts, or refusals are excluded from measurement, not treated as "brand absent."
- All formulas below are the only valid formulas for v1.1. Any deviation must be logged as a methodology change, not applied silently.

---

## 1. Mention Rate

| Field | Value |
|---|---|
| Level | Response-level |
| Definition | Share of eligible AI responses that contain an explicit mention of the brand (by name or unambiguous reference). |
| Numerator | Count of responses where `brand_mentioned = 1` |
| Denominator | Count of eligible responses (`response_valid = 1`) |
| Formula | `Mention Rate = (mentioned_responses / eligible_responses) × 100` |
| Unit | % |

---

## 2. Citation Rate

| Field | Value |
|---|---|
| Level | Response-level |
| Definition | Share of eligible responses containing at least one **qualifying** brand-associated citation. |
| Numerator | Count of responses where `citation_present = 1` |
| Denominator | Count of eligible responses |
| Formula | `Citation Rate = (cited_responses / eligible_responses) × 100` |
| Unit | % |

**Qualifying citation — inclusion rules:**
- Official brand website or domain ✅
- Verified company profile (LinkedIn, Crunchbase, industry directory) ✅
- Independent third-party article primarily about the brand ✅
- Brand mentioned inside a source that is not about the brand (passing reference) ❌
- AI states the brand name with no attributable source ❌ (counts toward Mention Rate, not Citation Rate)

---

## 3. Recommendation Rate

| Field | Value |
|---|---|
| Level | Query-level |
| Definition | Share of eligible *recommendation-type* queries in which the brand was recommended. |
| Numerator | Count of recommendation queries where `recommendation = 1` |
| Denominator | Count of eligible recommendation-category queries |
| Formula | `Recommendation Rate = (recommended_queries / eligible_recommendation_queries) × 100` |
| Unit | % |

Note: denominator is the recommendation query subset only — not all eligible responses. Do not reuse the Mention Rate denominator here.

---

## 4. Share of Voice (SOV)

v1.1 ships two variants. Only **Mention Share** is scored in AVI v1.1; **Prominence-weighted SOV** is documented for v1.2 and must not be silently substituted.

### 4.1 Mention Share (scored in v1.1)

| Field | Value |
|---|---|
| Level | Response-level, aggregated |
| Definition | Target brand's share of all qualifying brand mentions (target + tracked competitor set) across eligible responses. |
| Numerator | Total mention count of target brand |
| Denominator | Total mention count of target brand + all tracked competitors, across the same eligible response set |
| Formula | `Mention Share = (brand_mention_count / total_qualifying_mentions) × 100` |
| Unit | % |

### 4.2 Prominence-weighted SOV (deferred to v1.2 — not scored)

Rationale: a brand mentioned once in a 4-brand answer and a brand mentioned 7 times in the same slot are not equivalent. Simple Mention Share treats them identically. v1.2 will weight each mention by `prominence_score` (see §5) before aggregating. This is flagged here so the two are never confused in a report.

---

## 5. Prominence Score

| Field | Value |
|---|---|
| Level | Response-level |
| Definition | Normalized position/salience of the brand mention within a response that lists multiple entities. |
| Formula | `Prominence = 100 × (1 − (position − 1) / (total_entities_mentioned − 1))`, where `position` = the brand's rank/order in the response. If `total_entities_mentioned = 1`, `Prominence = 100`. |
| Range | 0–100 |
| Unit | Normalized score (not %) |

---

## 6. Framing

Framing is split into two distinct artifacts that must never be merged:

### 6.1 Framing Classification (categorical, not scored)
Each eligible response is labeled with one primary category: `Leader / Recommended / Neutral / Alternative / Warning / Negative`. This is a qualitative tag for reporting and pattern recognition — it carries no numeric weight in v1.1.

### 6.2 Framing Score (scored in v1.1)

| Field | Value |
|---|---|
| Level | Response-level, aggregated |
| Definition | Simple polarity-based score, replacing the arbitrary per-category weights used in the v1.0 draft. |
| Formula | `Framing Score = ((positive_responses + 0.5 × neutral_responses) / eligible_responses) × 100` |
| Where | `positive` = Leader / Recommended; `neutral` = Neutral / Alternative; `negative` = Warning / Negative (excluded from numerator) |
| Unit | % |

Mapping from category to positive/neutral/negative must be documented per project and version-locked before measurement — never adjusted after seeing results.

---

## 7. Cross-engine Consistency

| Field | Value |
|---|---|
| Level | Engine-level, aggregated |
| Definition | Degree to which the brand's visibility **status** (not raw variance) is reproduced across evaluated engines. |
| Numerator | Number of engines where the target signal is present (e.g., engine-level Mention Rate ≥ a predefined threshold, default 50%) |
| Denominator | Number of evaluated engines |
| Formula | `Cross-engine Consistency = (engines_with_signal_present / engines_evaluated) × 100` |
| Unit | % |

This deliberately replaces "variance across engines" from the v1.0 draft — variance alone cannot distinguish "consistently invisible" from "consistently visible" (both produce variance ≈ 0). The threshold used must be stated in every report.

---

## 8. Stability

| Field | Value |
|---|---|
| Level | Run-level, aggregated |
| Definition | Share of repeated measurements that fall within a predefined tolerance band of the baseline value. |
| Tolerance | Default: ±10% of baseline (must be stated if changed) |
| Numerator | Number of repeated runs within tolerance |
| Denominator | Total number of repeated runs |
| Formula | `Stability = (runs_within_tolerance / total_runs) × 100` |
| Unit | % |

This replaces "variance across runs" from the v1.0 draft — variance measures volatility, not stability, and the two are not interchangeable.

---

## 9. Summary Table

| Metric | Level | Numerator | Denominator | Unit |
|---|---|---|---|---|
| Mention Rate | Response | Mentioned responses | Eligible responses | % |
| Citation Rate | Response | Responses w/ qualifying citation | Eligible responses | % |
| Recommendation Rate | Query | Recommended queries | Eligible recommendation queries | % |
| Mention Share (SOV) | Response, aggregated | Brand mentions | Brand + competitor mentions | % |
| Prominence | Response | — (positional formula) | — | 0–100 |
| Framing Score | Response, aggregated | Positive + 0.5×Neutral | Eligible responses | % |
| Cross-engine Consistency | Engine | Engines with signal present | Engines evaluated | % |
| Stability | Run | Runs within tolerance | Total runs | % |
