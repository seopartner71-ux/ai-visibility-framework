# AI Visibility Framework

**Status:** v1.1 — Operational Methodology / Pilot

A practical methodology for measuring and improving brand visibility in generative AI search systems — Google AI Overviews, ChatGPT, Perplexity, Gemini, Yandex Neuro, and related engines.

**Author:** Vladimir Sinitsyn
**Organization:** Systemnoe SEO

---

## What this is

Traditional SEO answers the question *"where does the page rank?"* AI Visibility answers a different one: *"does the AI system understand, trust, and use information about this brand when it forms an answer?"*

This repository contains the full methodology: the conceptual framework, the measurement instruments (metrics, scoring, query set rules), the audit process, and supporting research artifacts — versioned so that a measurement taken today can be honestly compared to one taken months from now.

## Why "Pilot"

v1.1 is the first version of this methodology with fully specified, reproducible formulas. It has not yet been validated across a large multi-brand dataset. Treat scores and audits produced under v1.1 as a rigorous first assessment, not a benchmarked industry standard — that maturity comes with more pilot data, not with this version number alone.

---

## Repository structure

```
framework/
├── AI-Visibility-Framework-v1.1.md   — core concepts: Entity Authority, Semantic Coverage,
│                                        Content Quality, Source Trust, Citation Presence,
│                                        Structured Knowledge
├── AI-Readiness-Index-v1.1.md        — structural/technical readiness index (ARI)
└── AI-Visibility-Index-v1.1.md       — measured AI presence index (AVI)

methodology/
├── metric-definitions-v1.1.md        — every metric: numerator, denominator, unit, level
├── scoring-rubric-v1.1.md            — ARI/AVI weights, conversion formulas, worked example
├── query-set-methodology-v1.1.md     — how a query set is built, frozen, and reused
├── measurement-protocol-v1.1.md      — end-to-end steps for running a measurement
├── inter-rater-protocol-v1.1.md      — how multiple auditors reconcile scores
└── research-design-v1.1.md           — study design for the pilot program

audit/
├── AI-Visibility-Audit-v1.1.md       — the practical audit process (6 stages)
├── audit-template.md                 — fill-in template for a client audit
└── scoring-sheet.csv                 — raw scoring worksheet

research/
├── dataset-schema.md                 — field-by-field schema for measurement data
├── pilot-study-v1.1.md               — pilot program design and status
└── results.md                        — aggregated pilot findings

examples/
└── sample-audit.md                   — a fully worked, traceable example audit
```

## Where to start

- **New to the methodology?** Read `framework/AI-Visibility-Framework-v1.1.md` first.
- **Running an audit?** Go to `audit/AI-Visibility-Audit-v1.1.md`, then `audit-template.md`.
- **Implementing the scoring yourself?** Read `methodology/metric-definitions-v1.1.md` and `methodology/scoring-rubric-v1.1.md` together — the second depends entirely on the first.
- **Setting up a measurement?** `methodology/query-set-methodology-v1.1.md` and `research/dataset-schema.md` define the instrument before you run anything.

## Design principles

- Every published score ships with the raw-value table behind it. No score is reported without a way to recompute it from the dataset.
- Every metric has an explicit numerator and denominator. "Score" is never used as a standalone term — only as a qualified component (e.g. *Mention Score*, not *Score*).
- Response-level and query-level metrics use different denominators and are never mixed.
- The methodology does not claim to reveal how any AI system's internal ranking works. It evaluates observable external signals associated with how a digital entity is represented, understood, and connected to a topic.

## Status of this version

v1.1 is not a finished, fully validated standard — it is the first version where the math is fully specified and independently reproducible. Future versions will incorporate industry benchmarks, a larger case base, and comparative studies (see `research/pilot-study-v1.1.md`).

## License / Usage

Contact the author for usage terms, commercial application (AI Visibility Audit — Basic / Advanced / Consulting), or collaboration on the pilot program.
