---
name: jd-decode
description: Use when the user provides a job posting (URL or pasted text) to analyze, or asks to decode a JD, break down a posting, or build a job cluster.
---

# JD Decode

Turn one posting into a 30-minute posting analysis plus a cluster of similar roles. One decoded cluster feeds one sprint project that serves 5–15 applications.

## Recipe

1. Get the full JD: fetch the URL (board endpoint or scrape), or use pasted text.
2. Write the analysis with exactly these sections (template: `pipeline/clusters/_TEMPLATE.md`):
   - **Business problems this role solves** — 3–5 problem statements ("Finance leaders can't explain variance drivers fast enough for monthly close", not "does reporting")
   - **Five repeated skills/keywords** — verbatim from the JD
   - **Expected deliverables** — what this person ships in their first 90 days
   - **Industry and likely KPIs**
   - **AI-system angle** — what AI capability the role implies, if any
   - **Portfolio artifact** — the one thing that would prove the user can do part of this job
3. Cluster check: search for adjacent titles and list ≥3 similar open roles (company, title, link) this analysis also covers. If the posting is one-of-a-kind, say so and broaden the problem statement until it isn't.
4. Save to `pipeline/clusters/YYYY-MM-DD-<company>-<role-slug>.md` with the sections above plus the source link and cluster list.
5. Output the analysis and end with: run `/project-derive` to turn this cluster into the sprint project.

## Rules

- Problems are inferred, so phrase them as hypotheses grounded in JD language — never present them as inside knowledge of the company.
- Keywords must be verbatim; they feed resume tailoring in `/application-pack`.
