---
name: job-scout
description: Use when the user asks to find jobs, scan or scout postings, run the weekly scan, refresh the job tracker, or pick an anchor posting for the week.
---

# Job Scout

Weekly posting scan → scored shortlist → anchor posting. Source of truth: `pipeline/tracker.csv` (repo-relative). Plan rules: the user's filled-in plan (`PLAN.md` or `PLAN_TEMPLATE.md` at repo root).

<!-- CUSTOMIZE ─────────────────────────────────────────────
Fill these in (or the skill will read them from the plan file):
- LANES: your role lanes + % mix (from the plan's lane table)
- LOCATIONS: your metro(s) + "remote" as applicable
- SALARY_FLOOR: minimum when listed (unlisted is OK if title implies range)
- REFERRAL_NETWORK: which employer/community = automatic 2 on criterion 4
──────────────────────────────────────────────────────────── -->

## Recipe

1. Read the tracker to avoid re-adding roles and to surface overdue follow-ups (report these first).
2. **Poll the target list first** (`pipeline/target-companies.md`) — the primary source; fresher and more accurate than aggregators. For each company, hit its board directly with lane keywords, then keep only postings ≤30 days old (see `examples/job-search/docs/ATS_ENDPOINTS.md` for per-platform patterns):
   - **Workday** boards: POST to `https://{host}/wday/cxs/{tenant}/{site}/jobs` with `{"appliedFacets":{},"limit":20,"offset":0,"searchText":"<keywords>"}` — read `postedOn` for the date filter.
   - **Greenhouse:** `GET https://boards-api.greenhouse.io/v1/boards/{token}/jobs?content=true`.
   - **Lever:** `GET https://api.lever.co/v0/postings/{token}?mode=json`.
   - **Custom / SuccessFactors / UltiPro** boards: fetch the search-results URL with keyword params.
   - **Backlog companies** without a resolved endpoint: run one web search for "`{company}` careers job search workday", capture the board URL, and **append the resolved endpoint back into target-companies.md** so it's cached for next week.
   - Order: Tier 1 first, then Tier 2, then backlog as time allows. Surface home-metro roles at the top, but include remote/elsewhere per the plan's geography rules.
3. **Then supplement with an aggregator** (Indeed or similar, if a connector is available) for companies not on the target list. Aggregators return few results per query — run many queries: lane keywords × locations, weighted by the plan's lane shares.
4. Filter: keep only postings dated within the last 30 days (aggregators serve stale results — verify the posted date yourself). Apply the salary floor when salary is listed.
5. Fetch full details for survivors and score each 0–10 (plan rubric, 0–2 each): compensation/location workable; ≥60–70% of core requirements credible; domain overlap; a real referral or contact path exists (the user's strongest network = 2); existing experience or a project proves a relevant capability.
6. Append roles scoring ≥6 to the tracker, `status=scouted`. Do not add scores below 6.
7. Output: markdown table (company, title as the apply link, salary, posted, score) plus the anchor recommendation — highest scorer that also represents ≥3 similar open roles. End with: run `/jd-decode` on the anchor.

## Rules

- 8–10 = apply this week and seek a warm referral before or right after submitting; 6–7 = selective, only when the role supports a current project cluster; <6 = skip entirely.
- **First seven days of the plan:** add and score at least 20 fresh roles so skill/cert investments are evidence-based. Track which JD terms repeat for the after-20-roles review.
- Never fabricate salary or requirements-match — leave blank and say so.
- Keep apply URLs intact with all params, embedded on job titles.
