# Inter-Rater Protocol v1.1

**Status:** v1.1 — Operational Methodology / Pilot

## Purpose

Defines how multiple auditors independently scoring the same brand/dataset are expected to agree, and what happens when they don't. Required for the AI Readiness Index (ARI), where component scoring involves auditor judgment. The AI Visibility Index (AVI) is measurement-based and lower-risk here, but response classification fields (`framing_primary`, qualifying-citation calls) are still subject to this protocol.

## Auditor Selection

- Minimum two independent auditors per ARI assessment.
- Auditors must not have been involved in producing the content/assets being evaluated for the same brand (conflict of interest).
- Auditors work from the same published rubric (`scoring-rubric-v1.1.md`) and the same criteria checklist version.

## Independent Scoring

- Each auditor scores all ARI components and classifies all ambiguous AVI response-level fields (e.g. borderline qualifying citations) without consulting the other auditor.
- Scores are submitted and locked (timestamped) before any comparison takes place.

## Blind Assessment

- Auditors do not see each other's scores, notes, or classifications until both submissions are locked.
- Auditors do not see the client's own self-assessment (if one exists) before scoring.

## Disagreement Threshold

- ARI component-level disagreement of **more than 3 points (on the 0–20 or 0–15 scale, normalized to a 0–100 basis)** triggers reconciliation.
- AVI classification disagreement (e.g. qualifying citation yes/no, framing category) on **more than 10% of sampled responses** triggers reconciliation for that field across the full dataset, not just the disputed items.

## Reconciliation

- Auditors discuss flagged items only — not the full scoresheet — and document the reasoning for the final agreed value.
- If auditors cannot reach agreement, a third auditor scores the disputed items only, and the median of the three is used.
- All reconciliation decisions are logged with rationale, not just the final number — this log becomes part of the audit record.

## Reliability Metrics

- Report inter-rater agreement using a standard statistic appropriate to the data type (e.g. percentage agreement for categorical fields such as framing; intraclass correlation for continuous ARI component scores).
- Reliability statistics are published alongside the final AVI/ARI report, not only kept internally — this is what allows a client or third party to trust the number.

## Rubric Revision

- Any recurring source of disagreement (same component, same ambiguity type, across multiple audits) is logged as a candidate rubric clarification.
- Rubric clarifications are batched into the next versioned release (e.g. v1.2) — never patched silently mid-version, since that would break comparability with prior audits run under v1.1.
