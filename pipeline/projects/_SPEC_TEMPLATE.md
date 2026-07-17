# {{Project Name}} — Sprint Spec

**Label:** Independent role-aligned simulation using public/synthetic data
**Cluster served:** {{link to cluster file}} ({{N}} similar open roles)
**Sprint:** {{dates}}

## Problem statement

**One business question** (one, not ten):

> {{the question, tied to the anchor role and ≥3 similar roles}}

## Data

- **Source:** {{dataset + URL — official/public APIs first; no recognizable Kaggle staples}}
- **Access date:** {{YYYY-MM-DD}}
- **License/terms:** {{license}}
- **Data dictionary:** {{link or inline table before build starts}}

## Build plan (sprint shape)

| Days | Phase | Outcome |
|---|---|---|
| 1–2 | Scope + data | Question locked, data pulled, dictionary written |
| 3–5 | Build | {{core analysis / model / queries}} |
| 6–7 | Validate, document, publish | Assumptions checked, README + case study live |

> Not ready by day 7 → remove features and ship the smallest credible v1. Never carry a project past its sprint unless an active interview needs it.

## Acceptance checklist (all required before "published")

- [ ] One business question tied to anchor + ≥3 similar roles
- [ ] Data source, access date, license, data dictionary
- [ ] {{SQL/Python/other}} artifact with personally validated formulas and assumptions
- [ ] Most defensible deliverable for the problem: {{dashboard / memo / model / process map}}
- [ ] AI component **with validation** ({{narrative checked against computed values | classifier vs. baseline with reported metrics}}) — if relevant to the target roles
- [ ] Three insights + two recommendations
- [ ] README with screenshots
- [ ] `AI_USAGE.md` — honest disclosure of how AI tools were used
- [ ] Repo link added to tracker rows it serves
- [ ] One resume bullet
- [ ] 60-second verbal walkthrough rehearsed
- [ ] Two interview stories written down

## Validation step

> Non-negotiable: you personally validate every formula, query, assumption, and conclusion. AI can draft; you must be able to defend all of it live.

{{how you'll validate — recompute by hand, cross-check against a second source, etc.}}
