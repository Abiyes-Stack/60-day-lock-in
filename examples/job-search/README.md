# Worked Example: Job Search (Conversion archetype)

A complete generated lock-in system for the goal *"land a full-time analyst role (or complete 3 interviews) in 60 days."* This is the reference implementation `/lock-in-setup` studies before generating **your** system — and a ready-to-run kit if your goal happens to be a job search.

## The generated core loop

```
/job-scout → /jd-decode → /project-derive → /application-pack
  (find)      (decode)       (build)          (convert + refer)
    ↓            ↓               ↓                  ↓
tracker.csv   clusters/      projects/         packs/ + tracker update
```

The unit of work is a **job posting**; the insight that powers stage 1 is that most employers' boards expose public JSON you can poll directly (`docs/ATS_ENDPOINTS.md` — worth reading whatever your goal, as a model of the domain-specific depth a generated skill should reach).

## What's here

- `skills/` — the four pipeline skills. **Reference copies:** to use them live, copy each folder into the repo's `.claude/skills/` and fill in the `CUSTOMIZE` block at the top (your lanes, metro, salary floor, referral network). Or just tell `/lock-in-setup` your goal is a job search — it does exactly that adaptation from your interview answers.
- `pipeline/` — the tracker schema (headers + fictional example row), the target-companies list template with tiered ATS endpoints, and templates for every artifact: decoded clusters, sprint project specs, application packs.
- `docs/ATS_ENDPOINTS.md` — per-platform guide to polling Workday, Greenhouse, Lever, SuccessFactors, UKG, and iCIMS boards directly.
- `docs/SYSTEM.md` — this example as an agent-portable spec (hand it to any AI tool to operate the job-search pipeline without other context).

## How this maps to the framework

| Framework invariant | Job-search instantiation |
|---|---|
| Unit of work | One posting, one tracker row |
| Evidence week | Score ≥20 fresh postings; count repeating JD terms; cert/skill decisions trace to them |
| 0–10 rubric | Comp/location · requirements credibility · domain overlap · referral path · provability |
| Build sprints | Three portfolio projects, each derived from a posting cluster, each serving 5–15 applications |
| Quality bar | "Independent role-aligned simulation" label, validated formulas, `AI_USAGE.md`, two interview stories |
| 24-hour SLA | Recruiters, referrals, interview invitations |
| Warm paths | 30-contact list; a name beats a title beats "the team" |

Paths inside the skill files are written repo-root-relative (`pipeline/tracker.csv`) — they're correct once the skills are copied into live use at `.claude/skills/`.
