---
name: project-derive
description: Use when the user asks what to build this week, wants to derive or scope a portfolio project from a job cluster, or after a cluster file has been created.
---

# Project Derive

Turn a decoded job cluster into one sprint-sized portfolio project spec. Plan rules: the "Three portfolio sprints" section of the user's plan (repo root). Sprint dates come from the plan; after the third sprint comes a conversion window (no fourth project without an interview reason).

<!-- CUSTOMIZE ─────────────────────────────────────────────
- DATASET SOURCES: swap in official/public APIs for the user's field.
  Analyst/finance defaults: SEC Company Facts, USAspending, CFPB
  complaints, FDIC, Census MRTS, BTS FAF, EIA. Other fields: find
  the equivalent official open-data portals.
- ARTIFACT TYPES: SQL/Python for analyst roles; adjust per field.
──────────────────────────────────────────────────────────── -->

## Recipe

1. Read the cluster file — latest in `pipeline/clusters/` unless one is named. No cluster file → run `/jd-decode` first. The project must come from a current posting, never a preset topic.
2. Propose exactly 3 project options. Each option states:
   - **One business question** (one, not ten), tied to the anchor role and ≥3 similar roles
   - **Dataset** — official/public API first. Kaggle only when it fits the problem better AND is not a recognizable staple (no Titanic, telco churn, superstore, credit-default classics)
   - **Hard-skill artifact** — every project must produce one (SQL query set, Python analysis, etc.) that can be discussed in an interview
   - **AI component** — either narrative generation validated against computed values, or classification compared to a baseline with a reported accuracy/F1 (skip only if AI is irrelevant to the target roles)
   - **Deliverable** — a dashboard when that presentation serves the job problem; otherwise the most defensible memo, process map, or model
3. Have the user pick (AskUserQuestion) unless they already chose.
4. Write `pipeline/projects/YYYY-Wnn-<slug>/SPEC.md` from `pipeline/projects/_SPEC_TEMPLATE.md`:
   - Problem statement and the cluster it serves
   - Data source, **access date, license/terms**, and the label line: **"Independent role-aligned simulation using public/synthetic data"**
   - Build steps mapped to the sprint shape: scope + data on days 1–2, build on days 3–5, validate/document/publish on days 6–7, inside the plan's deep-work blocks
   - Acceptance standard as the done-checklist (full list in the spec template)

## Rules

- Never imply the simulation was commissioned by or used inside the company.
- The user personally validates formulas, queries, assumptions, and conclusions — the spec must include a validation step.
- Build not ready by day 7 → remove features and ship the smallest credible v1.
- Do not carry a project beyond its sprint unless it is directly needed for an active interview.
- One project serves 5–15 similar applications; don't derive a new project per company.
