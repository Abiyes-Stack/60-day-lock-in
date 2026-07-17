# The 60-Day Lock-In

**A goal-agnostic operating system for a 60-day sprint — that generates its own tooling for *your* goal.**

Give it a goal and your real context (hours, commitments, assets). It gives you back a personalized plan, a set of AI skills purpose-built for your goal's pipeline, and a tracker that makes progress undeniable. The framework is fixed; the tooling is generated.

```
your goal + your context
        │
        ▼
   /lock-in-setup  ──────────────►  PLAN.md            (your 60 days, scheduled)
   (the generator)                  .claude/skills/*   (3–5 skills for YOUR pipeline)
                                    pipeline/tracker   (your unit of work, scored)
        │
        ▼
   60 days of:  evidence week → three build sprints → conversion window
                weekly modes · Sunday resets · published proof · honest tracking
```

It works for **any goal with a deadline and a definition of done**: land a job, sign your first clients, launch a product, pass an exam, transform your fitness, ship an album. One complete worked example ships in the box: a full job-search system (`examples/job-search/`).

---

## Why this exists

Most 60-day "grind" plans fail the same three ways: they're built on intentions instead of evidence, they measure hours instead of outcomes, and they treat recovery as failure — so week 4 collapses. And generic productivity templates can't help you *execute*, because execution tooling has to know your goal.

This repo fixes both halves:

- **A fixed framework** (`framework/OPERATING_SYSTEM.md`) encoding what actually survives 60 real days: evidence-first decisions, three build sprints with hard scope-cut gates, outcome floors instead of hour counts, four operating modes including a legitimate Recovery mode, a tracker as the single source of truth, and a 24-hour SLA on inbound interest.
- **A generator** (`/lock-in-setup`) that interviews you, classifies your goal, and writes the goal-specific layer: the plan, the pipeline skills, the tracker schema. Your fitness lock-in and someone else's client-acquisition lock-in share a skeleton but nothing else.

## The three goal archetypes

The generator classifies your goal and builds the matching core loop:

| Archetype | Success means | Generated loop |
|---|---|---|
| **Conversion** | Someone says yes — job, clients, funding, admissions, bookings | scout opportunities → decode requirements → build targeted proof → convert + warm outreach → follow up |
| **Launch** | A thing ships to a real audience — product, book, channel, business | define audience/offer → build increments → ship gates → distribute → measure |
| **Progress** | A measured change in you — fitness, language, exam, craft | baseline → practice blocks → re-assessment gates → adjust → final assessment |

Blended goals (launch a freelance business = launch + conversion) get a blended system with one archetype leading.

---

## Setup

### Prerequisites

- Git and a GitHub account.
- [Claude Code](https://claude.com/claude-code) (or any capable AI agent — see "Other tools" below) for the generated-skills experience. **The framework also runs fully manually** — it's markdown and a CSV.
- Honest numbers about your available hours. The system is built for people with day jobs and real lives.

### With Claude Code (recommended)

```bash
# 1. Fork this repo on GitHub, then:
git clone https://github.com/<your-username>/60-day-lock-in.git
cd 60-day-lock-in

# 2. Open Claude Code and state your goal:
claude
> /lock-in-setup I want to sign my first 3 freelance design clients in 60 days
```

The generator will:

1. **Interview you** (two short rounds): dates, honest capacity, current assets, what "win" means — plus a fallback outcome you fully control, constraints to protect.
2. **Classify** your goal's archetype and design its core loop.
3. **Write your system:** `PLAN.md` at the root, 3–5 pipeline skills into `.claude/skills/` (immediately usable as slash commands), `pipeline/tracker.csv` with a schema for your unit of work, and templates for every artifact your pipeline produces.
4. **Hand you Week 1:** your exact first-seven-days checklist.

Then your loop is: run your generated skills through the week, and **`/weekly-reset` every Sunday** — it reads your tracker, reports actuals vs. your mode's floors, flags overdue follow-ups, recommends next week's mode, and picks the week's anchor.

### Privacy — read before your first commit

Your filled-in plan and tracker are your real life. Either **make your fork private**, or uncomment the privacy section in `.gitignore` (keeps `PLAN.md`, the tracker, and generated working data out of git while the framework stays versioned).

### Manual (no AI)

1. Read `framework/OPERATING_SYSTEM.md` (10 minutes — it's the whole philosophy).
2. Copy `PLAN_TEMPLATE.md` → `PLAN.md` and fill every placeholder.
3. Study `examples/job-search/` to see what a complete goal-specific system looks like, then hand-build your equivalents: a pipeline of 3–5 repeatable stage-checklists, a tracker CSV, artifact templates.
4. Run the weekly rhythm; do the Sunday reset from the checklist in your plan.

### Other AI tools (Codex, Cursor, etc.)

Hand your agent `docs/SYSTEM.md` — the agent-portable spec of the whole meta-system, including the generation contract. It can run setup, the weekly loop, or both.

---

## What's in the box

```
60-day-lock-in/
├── README.md                       ← you are here
├── PLAN_TEMPLATE.md                ← goal-agnostic plan skeleton (the generator fills it)
├── framework/
│   └── OPERATING_SYSTEM.md         ← the fixed core: phases, modes, tracker discipline,
│                                      quality bar, recovery protocol, final review
├── .claude/skills/
│   ├── lock-in-setup/              ← THE GENERATOR: goal + context → your system
│   └── weekly-reset/               ← universal Sunday reset (works with any generated system)
├── pipeline/                       ← empty until generated: your tracker + working files land here
├── docs/
│   └── SYSTEM.md                   ← agent-portable spec (for non-Claude tools)
└── examples/
    └── job-search/                 ← complete worked example (conversion archetype):
        ├── README.md                  4 skills, tracker, templates, and an ATS
        ├── skills/                    direct-polling guide that's worth reading
        ├── pipeline/                  even if you never job-hunt
        └── docs/
```

## The fixed framework, in one screen

- **Evidence week (days 1–7):** gather ≥20 scored real-world data points before committing to any study/build/buy decision. Act from day 1 anyway — evidence gathering isn't waiting.
- **Three 2-week build sprints (days 1–42):** each ships one published, defensible artifact. Scope days 1–2, build 3–5, validate/publish 6–7; the second week absorbs slip. Miss the gate → cut scope, ship the smallest credible version.
- **Conversion window (days 43–60):** double down on what earned attention. No fourth build without a conversion reason.
- **Operating modes:** Recovery / Core / Target / Stretch — outcome floors, chosen every Sunday, never more than two Stretch weeks in a row. Three good days out of five in Recovery counts as success.
- **The tracker is the source of truth.** One row per unit of work, 0–10 scored (five criteria × 0–2), hard thresholds: 8–10 act now, 6–7 selective, <6 skip.
- **Quality bar:** honest labeling, personally-validated cores, one question per artifact, a distribution kit (link + summary + 60-second walkthrough + two stories).
- **24-hour SLA** on all inbound interest. Outranks everything.
- **Day-60 review** answered from tracker evidence; the next 30-day lane is chosen from evidence, not anxiety.

Full version: [framework/OPERATING_SYSTEM.md](framework/OPERATING_SYSTEM.md).

## FAQ

**Can I run 30 or 90 days instead?** Yes — tell the generator. 30 = one sprint + conversion window; 90 = four sprints + mid-point review.

**What if my goal changes mid-sprint?** Re-run `/lock-in-setup`. It regenerates the goal layer; your framework, habits, and tracker history stay.

**Do I need the AI at all?** No. The framework is markdown and a CSV. The AI makes the goal-specific layer cheap to build and the Sunday reset effortless — that's it.

**Why 60 days?** Long enough to publish three real artifacts and convert on them; short enough that the deadline stays visceral.

## Contributing

PRs welcome — especially **new worked examples** under `examples/` (a launch-archetype and progress-archetype example are the most wanted), data-source lists by domain, and generator improvements. Keep personal data out of examples.

## License

MIT — see [LICENSE](LICENSE). Fork it, run it, share what you learn.
