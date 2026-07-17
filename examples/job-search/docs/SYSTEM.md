# System Spec — Job-to-Project Pipeline

> **Purpose of this file:** a self-contained, agent-portable specification of the whole system. Hand it to any AI tool (Claude Code, Codex, Cursor, a raw LLM) — or a very patient human — and they can operate the pipeline with no other context. The README covers *why*; this covers *exactly how*.

## 1. Objective and strategy

**Plan window:** 60 days, set in the owner's plan file (repo root).

**Core loop:** fresh job posting → business-problem decode → proof-of-work sprint → tailored application + warm outreach → follow-up → interview evidence. One project is designed to serve 5–15 similar applications, not one.

**Non-negotiables:**
- The tracker is the single source of truth for every role.
- All postings ≤30 days old when scouted.
- Recruiters, referrals, and interview invitations get replies within 24 hours.
- Never fabricate salary, requirements-match, or experience claims.
- LinkedIn is the referral/people-research layer, never a job-fetch source (it blocks scraping).

## 2. Directory layout

```
<repo root>/
├── PLAN.md (or PLAN_TEMPLATE.md)  # Master plan: rules, lanes, cadence, sprint dates
└── pipeline/
    ├── target-companies.md        # Curated company list + cached ATS endpoints
    ├── tracker.csv                # SOURCE OF TRUTH for all roles/status
    ├── clusters/                  # One file per decoded job cluster
    ├── projects/                  # One folder per sprint project
    └── packs/                     # One file per application
```

## 3. Data model — `tracker.csv`

Columns: `date_added, company, title, lane, location, salary, posted_date, fit_score, score_breakdown, url, status, contact_or_referral, applied_date, follow_up_date, project_link, notes`

- `salary` — as listed; blank if unlisted (never fabricate).
- `posted_date` — original posting date, verified from the board itself.
- `fit_score` / `score_breakdown` — 0–10 total, per-criterion `2/1/2/2/1` form (Section 5).
- `status` — `scouted → applying → applied → follow_up_sent → screen → interview → offer` / `rejected` / `stale`.
- `follow_up_date` — applied_date + 5–7 business days.

## 4. The four stages

### 4.1 job-scout — find and score roles

**Trigger:** "find jobs", "scout", "run the weekly scan", "refresh the tracker", "pick the anchor."

1. Read `tracker.csv` — avoid re-adding roles; surface overdue follow-ups first.
2. **Poll `target-companies.md` boards first** (fresher than aggregators). Per company, query its board with lane keywords; keep only postings ≤30 days:
   - Workday: `POST https://{host}/wday/cxs/{tenant}/{site}/jobs` body `{"appliedFacets":{},"limit":20,"offset":0,"searchText":"<kw>"}`; `postedOn` = date filter; `externalPath` = apply URL.
   - Greenhouse: `GET https://boards-api.greenhouse.io/v1/boards/{token}/jobs?content=true`.
   - Lever: `GET https://api.lever.co/v0/postings/{token}?mode=json`.
   - SuccessFactors / UKG / custom: fetch the search URL with keyword params (see `ATS_ENDPOINTS.md`).
   - Backlog companies: resolve the endpoint once, **cache it back into target-companies.md**.
   - Order: Tier 1 → Tier 2 → backlog.
3. Supplement with an aggregator for off-list coverage. Aggregators return few results per query and include stale postings — run many queries (lane keywords × locations, weighted by lane share) and verify dates.
4. Filter: ≤30 days; salary floor from the plan when listed.
5. Fetch full JDs for survivors; score 0–10 (Section 5).
6. Append roles ≥6 to the tracker with `status=scouted`. Never add <6.
7. Output: markdown table (company, title-as-apply-link, salary, posted, score) + **anchor recommendation** — the highest scorer that also represents ≥3 similar open roles. Hand off to jd-decode.

### 4.2 jd-decode — posting → problem analysis + cluster

**Trigger:** a posting URL / pasted JD, or "decode this", "build a cluster."

1. Get the full JD.
2. Write the analysis with exactly these sections:
   - **Business problems this role solves** — 3–5 concrete hypotheses grounded in JD language (never claimed as inside knowledge).
   - **Five repeated skills/keywords** — verbatim (they feed resume tailoring).
   - **Expected deliverables** — what this person ships in 90 days.
   - **Industry and likely KPIs.**
   - **AI-system angle** — the AI capability the role implies (if any).
   - **Portfolio artifact** — the one thing that proves the owner can do part of the job.
3. **Cluster check:** find ≥3 similar open roles (company, title, link). One-of-a-kind → broaden the problem statement until it isn't.
4. Save to `clusters/YYYY-MM-DD-<company>-<role-slug>.md`. Hand off to project-derive.

### 4.3 project-derive — cluster → sprint project spec

**Trigger:** "what should I build", "derive/scope a project", or a fresh cluster file.

1. Read the latest (or named) cluster file. The project must come from a current posting, never a preset topic.
2. Propose exactly 3 options; each states: one business question tied to anchor + ≥3 roles; dataset (official/public API first; Kaggle only if it fits better AND isn't a recognizable staple); a hard-skill artifact discussable in an interview; an AI component (validated narrative generation OR classification vs. baseline with reported metrics); the deliverable (dashboard when it serves the job problem, else the most defensible memo/process map/model).
3. Owner picks.
4. Write `projects/YYYY-Wnn-<slug>/SPEC.md`: problem + cluster served; data source + access date + license + the label **"Independent role-aligned simulation using public/synthetic data"**; build steps (scope+data days 1–2, build 3–5, validate/publish 6–7); the acceptance checklist (Section 6).

### 4.4 application-pack — role → tailored materials + referral

**Trigger:** ready to apply / "give me materials for this role."

1. Gather: full JD, matching cluster file, most relevant project link, owner's provable background.
2. Produce exactly: **tailored summary** (2–3 sentences, market identity, never "aspiring"); **3–5 resume bullets** (verbatim cluster keywords mapped to real experience); **project link** (one relevance line); **warm outreach** (<120 words, role named, one hook, one small ask, one or two *credible* contacts, strongest network routed first); **follow-up date** (+5–7 business days).
3. Save to `packs/YYYY-MM-DD-<company>-<slug>.md`; update the tracker row.

## 5. Scoring rubric (0–10; 0–2 per criterion)

1. Compensation and location workable.
2. ≥60–70% of core requirements credible.
3. Domain overlap with the owner's experience.
4. Real referral/contact path (strongest network = 2).
5. Existing experience or a project proves a relevant capability.

**Thresholds:** 8–10 apply this week + warm referral; 6–7 selective (must support a current cluster); <6 skip, don't track.

**First seven days:** score ≥20 fresh roles; count repeating JD terms — skill/cert investments must trace to this evidence.

## 6. Project rules and acceptance standard

**Rules:** public/synthetic/licensed data only; never imply the simulation was commissioned by or used inside the company; always label it; one business question; AI may plan/code/debug/document, but the owner personally validates every formula, query, assumption, and conclusion; one project serves 5–15 applications; sprint overrun → cut features, ship smallest credible v1.

**Acceptance (all required):** one business question tied to anchor + ≥3 roles; data source with access date, license, data dictionary; validated hard-skill artifact; most defensible deliverable; AI component with validation; three insights + two recommendations; README with screenshots; `AI_USAGE.md`; project link in tracker; one resume bullet; 60-second walkthrough; two interview stories.

## 7. Weekly cadence

Daily: one protected deep block + one themed evening block; income source protected; one theme per day.

Weekly: Mon scout/apply + outreach · Tue hard-skill + study · Wed project + parallel track · Thu project + applications/follow-ups · Fri follow-ups/polish + recovery · Sat project or parallel track · Sun 30–45-min reset (tracker, follow-ups, calendar, next anchor).

Modes (owner picks each Sunday): Recovery 3 applications · Core 5 + 5–8 warm messages · Target 8 + 8–12 · Stretch 12–15 (max two consecutive weeks). Track outcomes, not hours.

## 8. Tooling portability

| Function | Works with | Notes |
|---|---|---|
| Board polling | `curl` / any HTTP tool | The Workday CXS POST is the most reusable primitive (Section 4.1) |
| Aggregator search | Any job-API connector, or manual | Verify posting dates yourself |
| JD fetching | HTTP fetch or scraper | Custom portals may need scraping |
| Tracker | Any CSV editor / spreadsheet | CSV works everywhere; DB/Notion optional |
| People research | LinkedIn (manual) | Never for job-fetching |
| Outreach + follow-ups | Any mail/messaging client | The 24-hour SLA is the rule that matters |

**Known limitations:** aggregators are sparse and stale; LinkedIn blocks scraping; salary often unlisted (record blank); ATS endpoints move on migration (re-resolve and re-cache).
