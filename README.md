# The 60-Day Lock-In

**A forkable operating system for a 60-day career sprint — driven by real job postings, not guesswork.**

Most job searches run backwards: build a generic portfolio, blast untailored applications, hope something sticks. This system inverts that. Live job postings tell you *what to build*, and every project you ship serves 5–15 tailored applications backed by warm outreach.

The core loop:

```
fresh job posting → business-problem decode → proof-of-work sprint
      → tailored application + warm outreach → follow-up → interview evidence
```

Everything in this repo exists to run that loop for 60 days without burning out.

---

## Who this is for

- Career changers and early/mid-career professionals targeting analyst, engineering, data, finance, operations, or similar roles.
- Anyone juggling a job search alongside a day job, side business, or family commitments (the system assumes limited hours and protects recovery).
- People who want their portfolio to *prove* they can do a specific job, instead of showcasing tutorial projects hiring managers have seen a thousand times.

You do **not** need any AI tooling to run this — the whole system works with a text editor and a spreadsheet. But if you use [Claude Code](https://claude.com/claude-code) (or any capable AI agent), this repo ships four ready-made skills that automate the heavy lifting.

---

## What's in the box

```
60-day-lock-in/
├── README.md                      ← you are here (setup + full system guide)
├── PLAN_TEMPLATE.md               ← the customizable 60-day plan — fill this in first
├── LICENSE                        ← MIT; fork freely
├── pipeline/
│   ├── README.md                  ← quick-start for the weekly pipeline
│   ├── tracker.csv                ← THE source of truth for every role (template + example row)
│   ├── target-companies.md        ← your curated company list + ATS board endpoints (template)
│   ├── clusters/_TEMPLATE.md      ← decoded job-cluster format
│   ├── projects/_SPEC_TEMPLATE.md ← sprint project spec format
│   └── packs/_TEMPLATE.md         ← application pack format
├── docs/
│   ├── SYSTEM.md                  ← the full agent-portable system spec (hand it to any AI tool)
│   └── ATS_ENDPOINTS.md           ← how to poll company job boards directly (the secret weapon)
└── .claude/skills/                ← four Claude Code skills, auto-loaded when you open
    ├── job-scout/                    Claude Code inside this repo
    ├── jd-decode/
    ├── project-derive/
    └── application-pack/
```

---

## How the system works

### The four-stage pipeline

| Stage | What it does | Output |
|---|---|---|
| **1. Job scout** | Weekly scan of your target companies' boards + aggregators. Scores every role 0–10 against a fixed rubric. Picks an "anchor" posting. | Scored shortlist → `pipeline/tracker.csv` |
| **2. JD decode** | Reverse-engineers the anchor posting into the *business problems* the role solves, verbatim keywords, KPIs, and expected deliverables. Confirms ≥3 similar open roles exist (a "cluster"). | Analysis file → `pipeline/clusters/` |
| **3. Project derive** | Turns the cluster into ONE sprint-sized portfolio project spec: one business question, a public dataset, a hard-skill artifact, and clear acceptance criteria. | Project spec → `pipeline/projects/` |
| **4. Application pack** | Per role: tailored summary, 3–5 resume bullets mirroring the JD's verbatim keywords, a warm-outreach message under 120 words, and a follow-up date. | Pack file → `pipeline/packs/` + tracker update |

One decoded cluster feeds one project. One project serves **5–15 similar applications**. That's the leverage.

### The tracker is the source of truth

Every role lives as one row in `pipeline/tracker.csv` with its fit score, status, contact route, project link, and follow-up date. If it isn't in the tracker, it didn't happen.

Status flow: `scouted → applying → applied → follow_up_sent → screen → interview → offer` (or `rejected` / `stale`).

### The scoring rubric (0–10)

Score every candidate role 0–2 on each of five criteria:

1. **Compensation and location are workable** for you.
2. **At least 60–70% of core requirements are credible** — you can defend them in an interview.
3. **Domain overlap** — the role touches your existing experience (your fields go here when you customize).
4. **A real referral or contact path exists** (your strongest network = automatic 2).
5. **Existing experience or a project can prove a relevant capability.**

Action thresholds:
- **8–10:** apply this week; seek a warm referral before or immediately after submission.
- **6–7:** apply selectively, only when the role supports a current project cluster.
- **Below 6:** skip. Do not spend prime time manufacturing fit. Don't even add it to the tracker.

---

## Setup — step by step

### Step 0: Prerequisites

- Git and a GitHub account (your projects get published there — the portfolio *is* the point).
- A text editor. Optionally [Claude Code](https://claude.com/claude-code) or another AI agent for the automated skills.
- 10–12 focused hours per week. The plan is built for people with day jobs.

### Step 1: Fork and clone

```bash
# Fork this repo on GitHub first (top-right "Fork" button), then:
git clone https://github.com/<your-username>/60-day-lock-in.git
cd 60-day-lock-in
```

> **Privacy note:** once you start filling in the tracker and contact lists, your fork contains your job hunt. Either make your fork **private** (GitHub → Settings → change visibility), or keep working data out of git — `.gitignore` has a ready-made "privacy mode" section you can uncomment.

### Step 2: Fill in your plan

Open `PLAN_TEMPLATE.md` and work top to bottom. Every `{{PLACEHOLDER}}` is a decision:

1. **Dates** — pick a start date; Day 60 falls out automatically.
2. **Primary outcome** — one measurable sentence. Example: *"Land a full-time data analyst role at $X+ or complete at least three interviews by Day 60."* An outcome you don't fully control (offer) needs a fallback you do (interviews completed).
3. **Market identity** — one sentence you lead with everywhere. Never "aspiring." Every word must be provable.
4. **Role lanes** — 3–6 role families with a % application mix. Lanes stop you from being "open to anything," which reads as qualified for nothing.
5. **Daily blocks** — map your real day. Protect your income source, your sleep, and at least one recovery block. The template assumes a morning deep block + one evening block.
6. **Parallel tracks** — optional modules for a side business, certifications, or life admin. Cut anything that doesn't serve income or conversion.

### Step 3: Build your target-company list

Open `pipeline/target-companies.md`:

1. List 10–20 companies you'd genuinely work for, tiered: **Tier 1** = strongest referral network / home market, **Tier 2** = target metros and sectors, **Backlog** = resolve later.
2. For each, resolve its ATS job-board endpoint once and cache it — **read `docs/ATS_ENDPOINTS.md`**, it's the highest-leverage 10 minutes in this setup. Most employers run one of ~6 ATS platforms (Workday, Greenhouse, Lever, SuccessFactors, UKG, custom), and most expose structured JSON you can poll directly with `curl` — fresher and more complete than any aggregator.

### Step 4: Set up the tracker

`pipeline/tracker.csv` ships with headers and one fictional example row. Delete the example, keep the headers. Open it in any spreadsheet app, or let the AI skills maintain it.

### Step 5 (optional): Wire up the AI skills

If you use Claude Code, the four skills in `.claude/skills/` load automatically when you open Claude Code inside this repo:

```bash
cd 60-day-lock-in
claude
# then: /job-scout        → run the weekly scan
#       /jd-decode        → decode an anchor posting
#       /project-derive   → scope this sprint's project
#       /application-pack → generate tailored materials for a role
```

Before first use, open each `SKILL.md` and fill in the `CUSTOMIZE` block at the top (your lanes, your metro, your referral network). The skills read your plan and tracker from this repo's relative paths.

Using a different agent (Codex, Cursor, a raw LLM)? Hand it `docs/SYSTEM.md` — it's a self-contained spec written to be operated without any other context.

No AI at all? Each skill file doubles as a manual checklist. The system predates the automation.

### Step 6: Run Week 1

Day-by-day for the first seven days (also in the plan template):

| Day | Actions |
|---|---|
| 1 | Set calendar blocks for all 60 days. Confirm the tracker is your sole application record. List your top 30 warm contacts. |
| 2 | First job scan; score your first batch of roles. Update your LinkedIn headline/About to match your market identity. |
| 3 | Complete a 20-role scored review. Pick your Project 1 anchor posting and decode it. Choose the dataset and write the 7-day project spec. |
| 4 | Build the project's data dictionary and first analysis artifact. Send 2–4 warm messages. Sunday-style reset. |
| 5 | One hard-skill practice block. Build the project core. Submit your first tailored application. |
| 6 | Second practice block. Continue project build. |
| 7 | Validate the project's assumptions. Draft README, resume bullet, and 60-second explanation. |

**The first-week rule: add and score at least 20 fresh roles.** Those 20 JDs are your market evidence — which skills repeat, which certifications actually appear, which lanes are hiring. Every later decision (what to study, what to build) should trace back to this evidence, not vibes.

---

## The weekly operating system

### Weekly rhythm (customize in the plan template)

| Day | Morning deep block | Evening primary block |
|---|---|---|
| Mon | Scout roles / select priority applications | Tailor + submit applications; warm outreach |
| Tue | Hard-skill practice (SQL, Python, etc.) | Certification or study block |
| Wed | Project build | Side-business / parallel track |
| Thu | Project build | Applications, follow-ups, interview prep |
| Fri | Follow-ups, profile polish | Recovery (or live interviews only) |
| Sat | Project build or parallel track | Event / life / recovery |
| Sun | 30–45 min reset: tracker, follow-ups, calendar, next anchor | Optional 90-min completion block |

Assign each day ONE theme. Do not schedule every priority every day.

### Operating modes — track outcomes, not hours

Pick your mode every Sunday. Missing a day never means doubling the next one.

| Metric | Recovery | Core (normal) | Target |
|---|---:|---:|---:|
| High-fit applications | 3 | 5 | 8 |
| Warm outreach messages | 0–2 | 5–8 | 8–12 |
| Follow-up sessions | 1 | 2 | 2 |
| Hard-skill practice blocks | 1 | 2 | 2–3 |
| Portfolio progress | one meaningful step | sprint milestone | published project |
| Exercise | 2 sessions | 2+ | 2+ |

There's also a **Stretch** mode (12–15 applications) — allowed only with prepared materials, real follow-up capacity, and never more than two consecutive weeks.

### Recovery protocol

When a day is missed:

1. Do **not** move every missed task forward.
2. Return to the next scheduled themed block.
3. Protect sleep, your income source, and interview responsiveness above all else.
4. Complete the smallest action that restores momentum: one application, one message, one 30-minute block.
5. Review on Sunday; deliberately choose next week's mode.

**Response SLA:** recruiters, referrals, and interview invitations get replies within 24 hours. This outranks everything else in the system.

---

## The three portfolio sprints

Days 1–14, 15–28, and 29–42 are two-week build sprints; the remainder is a **conversion window** (improve whichever project earns attention; no fourth project without an interview reason).

Within each sprint: **scope + data on days 1–2, build on days 3–5, validate/document/publish on days 6–7** (the second week absorbs slip and application work). Not ready by day 7? Remove features and ship the smallest credible v1.

### Project acceptance standard

Before a project is "published," it needs every item on this list:

- [ ] **One business question** tied to the anchor role and ≥3 similar open roles
- [ ] Data source with **access date, license/terms, and a data dictionary**
- [ ] A hard-skill artifact (SQL, Python, etc.) with **personally validated** formulas and assumptions
- [ ] The most defensible deliverable for the problem — dashboard, memo, model, or process map
- [ ] One AI component *with validation* (e.g., generated narrative checked against computed values, or a classifier compared to a baseline with reported metrics) — if AI is relevant to your target roles
- [ ] Three insights and two recommendations
- [ ] README with screenshots + an `AI_USAGE.md` disclosing how AI tools were used
- [ ] One resume bullet, a 60-second verbal walkthrough, and **two interview stories**
- [ ] The label: *"Independent role-aligned simulation using public/synthetic data"*

That label matters. Never imply a project was commissioned by or used inside a company. Recruiters respect the honesty; they've seen the alternative.

**Dataset rules:** official/public APIs first (government data portals, SEC, census bureaus, industry open data). Kaggle only when it genuinely fits AND isn't a recognizable staple — no Titanic, no telco churn, no superstore. A hiring manager who has seen your dataset a hundred times has already stopped reading.

---

## The ATS insight (why this beats job boards)

The single most reusable trick in this system: **most companies' career sites are thin frontends over ~6 ATS platforms, and most of those expose public JSON endpoints.**

```bash
# Example — any Workday-hosted board:
curl -X POST "https://{host}/wday/cxs/{tenant}/{site}/jobs" \
  -H "Content-Type: application/json" \
  -d '{"appliedFacets":{},"limit":20,"offset":0,"searchText":"financial analyst"}'
```

That returns clean, paginated JSON with posting dates — no scraping, no auth, no stale aggregator listings. Resolve each target company's endpoint once, cache it in `pipeline/target-companies.md`, and your weekly scan becomes a two-minute loop. Full guide with per-platform patterns (Workday, Greenhouse, Lever, SuccessFactors, UKG): **[docs/ATS_ENDPOINTS.md](docs/ATS_ENDPOINTS.md)**.

Aggregators (Indeed, LinkedIn Jobs) become *supplements* for off-list discovery — and always verify posting dates yourself; they serve stale listings. LinkedIn's job in this system is **people research for warm outreach**, never job-fetching.

---

## Warm outreach rules

The gap for most people is activation, not access.

- Build a list of 30 warm contacts on Day 1 — former colleagues, alumni, community connections, anyone at a target company.
- For each priority role, find one or two people with a *credible* connection. Never force two messages when only one relationship is real.
- Keep messages **under 120 words**: name the role, one specific hook (your project or shared background), one small clear ask — insight, a 10-minute conversation, a resume referral, or "who's the best person to talk to?"
- A name beats a title; a title beats "the team."
- Log every message, reply, and outcome in the tracker.

---

## Day-60 review

On the last day, answer from tracker evidence — not memory:

- Which title/industry/location combinations produced screens or interviews?
- Which resume version and which project link converted best?
- Did referrals outperform cold applications? (They will. By how much?)
- Which skill gaps repeatedly appeared in interviews?
- What is the evidence-based next 30-day specialization?

Select the next lane from evidence, not anxiety or novelty.

---

## Customizing further

- **Different field?** The pipeline is role-agnostic. A designer's "project" is a case study; a developer's is a shipped feature; a marketer's is a campaign analysis. Only the acceptance standard's artifact types change.
- **Shorter sprint?** Compress to 30 days: one project sprint + conversion window. Keep the weekly rhythm intact.
- **No side business?** Delete the parallel-track sections from the plan. The template marks every optional module.
- **Different AI tooling?** `docs/SYSTEM.md` is written to be handed to any agent as a standalone spec.

## Contributing

PRs welcome — especially new ATS endpoint patterns for `docs/ATS_ENDPOINTS.md`, dataset sources by field, and translations of the plan template. Keep personal data out of examples.

## License

MIT — see [LICENSE](LICENSE). Fork it, run it, share what you learn.

---

*Built by someone running the system for real, mid-sprint. The tracker rows in the example files are fictional; the method isn't.*
