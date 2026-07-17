# Job-to-Project Pipeline

The weekly flow. Each stage has a Claude Code skill in `.claude/skills/` (auto-loaded when you open Claude Code in this repo) and doubles as a manual checklist.

1. **`/job-scout`** — weekly scan. Polls your `target-companies.md` boards first (see `docs/ATS_ENDPOINTS.md`), then aggregators. Keeps postings ≤30 days old, scores each 0–10, writes ≥6 scorers to `tracker.csv`, recommends the anchor posting.
2. **`/jd-decode`** — decode the anchor into business problems, verbatim keywords, KPIs, deliverables, and a cluster of ≥3 similar roles. Saves to `clusters/`.
3. **`/project-derive`** — turn the cluster into a sprint project spec (scope days 1–2, build days 3–5, publish days 6–7) with a public dataset and one business question. Saves to `projects/`.
4. **`/application-pack`** — per role: tailored summary, bullets mirroring verbatim JD keywords, warm-outreach message, follow-up date. Saves to `packs/`, updates the tracker.

**Source of truth:** `tracker.csv`. Plan rules: `../PLAN_TEMPLATE.md` (your filled-in copy).

**Status values:** `scouted → applying → applied → follow_up_sent → screen → interview → offer` / `rejected` / `stale`.

## tracker.csv columns

| Column | Meaning |
|---|---|
| `date_added` | When the role entered the tracker |
| `company` / `title` / `location` | The role |
| `lane` | Which of your plan lanes it belongs to |
| `salary` | As listed; **blank if unlisted — never fabricate** |
| `posted_date` | Original posting date (≤30 days at scan time) |
| `fit_score` | 0–10 from the rubric |
| `score_breakdown` | Per-criterion, e.g. `2/1/2/2/1` |
| `url` | Apply link, kept intact with all params |
| `status` | See flow above |
| `contact_or_referral` | Named person or referral route |
| `applied_date` / `follow_up_date` | Follow-up = applied + 5–7 business days |
| `project_link` | The portfolio project used as proof |
| `notes` | Outreach dates, replies, next actions |
