# Scoring Rubric v1.1

**Status:** v1.1 — Operational Methodology / Pilot
**Depends on:** `metric-definitions-v1.1.md`

This document defines exactly how raw metrics convert into the two composite indices: **AI Readiness Index (ARI)** and **AI Visibility Index (AVI)**. Both replace the informal term "Score" used in the v1.0 draft — "Score" is reserved for individual component values only, never for the standalone term without qualifier.

---

## 1. AI Readiness Index (ARI) — 0–100

ARI evaluates structural/technical readiness (Entity Authority, Semantic Coverage, Content Quality, Source Trust, Citation Presence readiness, Structured Knowledge). It is assessed by an auditor against defined criteria per component, not derived from AI-response measurement.

| Component | Weight |
|---|---|
| Entity Authority | 20 |
| Semantic Coverage | 20 |
| Content Quality | 20 |
| Source Trust | 15 |
| Citation Presence (readiness) | 15 |
| Structured Knowledge | 10 |
| **Total** | **100** |

**Rule:** Intermediate scores are allowed and expected — e.g. `Entity Authority = 13.5/20`. Forcing auditors into round numbers produces artificial score clustering and reduces inter-rater reliability. Each component's criteria checklist lives in a separate scoring sheet; this file only fixes the weights and the intermediate-scoring rule.

---

## 2. AI Visibility Index (AVI) — 0–100

AVI is derived from the measured metrics defined in `metric-definitions-v1.1.md`. It is a **measurement-based** index — every component score must be traceable back to a raw value.

| Component | Raw metric | Weight |
|---|---|---|
| Mention | Mention Rate | 15 |
| Citation | Citation Rate | 15 |
| SOV | Mention Share | 20 |
| Prominence | Prominence (normalized) | 15 |
| Recommendation | Recommendation Rate | 15 |
| Framing | Framing Score | 10 |
| Consistency | Cross-engine Consistency | 5 |
| Stability | Stability | 5 |
| **Total** | | **100** |

### 2.1 Conversion formula

For any metric expressed as a percentage (Mention Rate, Citation Rate, Mention Share, Recommendation Rate, Framing Score, Consistency, Stability):

```
Component Score = Raw Metric (%) × Weight / 100
```

For metrics expressed as a 0–100 normalized score (Prominence only, in v1.1):

```
Component Score = Normalized Score × Weight / 100
```

**No other conversion is valid in v1.1.** If a future version changes this (e.g. introducing non-linear scaling), it must be versioned as v1.2 and documented as a breaking change.

---

## 3. Worked Example (corrected)

This replaces the informal `~64/100` figure that appeared in the v1.0 draft. Using the same illustrative raw values that produced the discrepancy, the correct, fully-traceable result is:

| Metric | Raw | Weight | Component Score |
|---|---|---|---|
| Mention | 55% | 15 | 55 × 15 / 100 = **8.25** |
| Citation | 22% | 15 | 22 × 15 / 100 = **3.30** |
| SOV (Mention Share) | 18% | 20 | 18 × 20 / 100 = **3.60** |
| Prominence | 40 (normalized) | 15 | 40 × 15 / 100 = **6.00** |
| Recommendation | 20% | 15 | 20 × 15 / 100 = **3.00** |
| Framing | 70% | 10 | 70 × 10 / 100 = **7.00** |
| Consistency | 60% | 5 | 60 × 5 / 100 = **3.00** |
| Stability | 80% | 5 | 80 × 5 / 100 = **4.00** |
| **AVI Total** | | **100** | **38.15 / 100** |

Reported result: **AVI = 38.15/100** — never `~64/100` or any unrounded/unsourced figure. Every published score must ship with its raw-value table attached (or linked), so it can be independently recomputed from the dataset.

### 3.1 Reading the result against the Maturity Model

38.15/100 maps to **Level 1 — Recognized** in the Maturity Model (21–40 band): the brand exists online, but the connection between company, experts, and topic is weak. This is the kind of statement AVI should support directly — a number that maps cleanly to an actionable maturity band, not a decorative score.

---

## 4. What v1.1 deliberately does not do

- Does not weight Framing categories subjectively (see `metric-definitions-v1.1.md` §6.2 — replaced with a documented positive/neutral/negative polarity formula).
- Does not compute Cross-engine Consistency or Stability as raw variance (see §7–8 of the same file).
- Does not apply Prominence-weighted SOV — that is explicitly deferred to v1.2 and must not appear in any v1.1 report.
