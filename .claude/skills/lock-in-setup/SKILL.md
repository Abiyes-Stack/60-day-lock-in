---
name: lock-in-setup
description: Use when the user states a goal they want to pursue with the 60-day lock-in system, asks to set up / customize / initialize their lock-in, or runs this skill with a goal as the argument. Generates the personalized plan, the goal-specific pipeline skills, and the tracker for THEIR goal.
---

# Lock-In Setup — the generator

Turn one stated goal + the user's real context into a complete, personalized 60-day operating system: a filled-in plan, 3–5 goal-specific pipeline skills written into `.claude/skills/`, and a tracker schema. The framework is fixed; everything goal-shaped is generated here.

**Reference implementation:** `examples/job-search/` is a complete generated system for one goal ("land an analyst role in 60 days"). Study it before generating — every system you produce should reach that level of specificity: concrete recipes, real data sources, hard rules, exact file paths.

## Recipe

### 1. Capture the goal

Take the goal from the user's message or skill argument. If it's vague ("get fit", "make money"), sharpen it into one measurable sentence before proceeding — a goal that can't be scored on Day 60 can't drive a system.

### 2. Interview for context (AskUserQuestion, 2 rounds max)

Gather only what generation needs:

- **Deadline & dates** — start date; 60 days is the default, but accept 30/90.
- **Capacity** — honest hours/week, which hours (mornings? evenings?), and the fixed commitments that must be protected (job, family, recurring events).
- **Current assets** — relevant skills, work, audience, network, savings, equipment. What's provable today.
- **Definition of win** — the primary outcome, plus a fallback outcome the user fully controls (you can't force an offer/client/PR; you can force quality volume).
- **Constraints & parallel tracks** — income sources to protect, side commitments with deadlines, health non-negotiables.

Don't interrogate. Two AskUserQuestion rounds, then infer sensibly and state your assumptions in the generated plan.

### 3. Classify the goal archetype

| Archetype | Signature | Core loop to generate |
|---|---|---|
| **Conversion** | Success = someone else says yes (job, clients, funding, admissions, gigs, publishing deals) | scout opportunities → decode what each really requires → build targeted proof → convert (apply/pitch) + warm outreach → follow up → evidence |
| **Launch** | Success = a thing ships to a real audience (product, album, book, channel, business) | define audience/offer → build in public-ready increments → ship gates → distribute → measure → iterate |
| **Progress** | Success = a measured change in the user (fitness, language, exam, skill mastery) | baseline assessment → practice blocks → periodic re-assessment gates → adjust program → final assessment |

Real goals often blend (launch a freelance business = launch + conversion). Generate for the blend, with one archetype leading.

### 4. Generate the system

Write these files, in this order:

**a. `PLAN.md`** (repo root) — fill `PLAN_TEMPLATE.md` completely with the user's answers. No `{{placeholders}}` may survive. Daily blocks map to their real hours; parallel tracks get bounded blocks and delivery gates or are deleted; sprint dates are computed from the start date.

**b. 3–5 pipeline skills** in `.claude/skills/<skill-name>/SKILL.md` — the stages of THEIR core loop. Every generated skill must have:
- Frontmatter: `name` (kebab-case, domain-specific — `client-scout` not `stage-one`) and a `description` starting "Use when the user…" with concrete trigger phrases.
- A one-line purpose statement naming its inputs and outputs with exact repo-relative paths.
- A numbered **Recipe** — concrete steps an agent can execute, including real data sources, real endpoints or search strategies for the domain, output file formats, and the handoff line to the next stage's skill.
- A **Rules** section — the hard constraints: honesty rules (never fabricate numbers/claims), freshness windows, quality floors, when to skip.

**c. `pipeline/tracker.csv`** — headers + one clearly-marked fictional example row. The unit of work is the goal's atom (conversion: one opportunity; launch: one shippable increment; progress: one assessment/session). Always include: date, status (define the full status flow in `pipeline/README.md`), a 0–10 fit/priority score with per-criterion breakdown, a link/evidence column, and a follow-up or next-action date.

**d. `pipeline/README.md`** — the weekly flow in order, which skill runs when, the status flow, and the tracker column definitions.

**e. Templates** in `pipeline/` subfolders — one `_TEMPLATE.md` per artifact type the skills produce (mirror how `examples/job-search/pipeline/` does it).

### 5. Adapt the universal invariants

These survive into EVERY generated system, rephrased for the domain:

1. The tracker is the single source of truth; if it isn't tracked, it didn't happen.
2. **Evidence week:** the first 7 days gather ≥20 scored data points (opportunities, market signals, baseline measurements) — later investment decisions must trace to this evidence, not vibes.
3. A 0–10 scoring rubric with five 0–2 criteria and hard thresholds (act on 8–10; selective on 6–7; skip <6).
4. Three ~2-week build sprints, then a conversion/consolidation window. Sprint overrun → cut scope, ship the smallest credible version.
5. An acceptance standard per published artifact — including personal validation of anything AI-assisted, and honest labeling (simulations labeled as simulations; no fabricated metrics, testimonials, or claims).
6. Operating modes (Recovery / Core / Target / Stretch) with outcome floors, chosen every week — outcomes, not hours.
7. The recovery protocol: never roll everything forward; smallest momentum-restoring action; protect sleep, income, and responsiveness.
8. A 24-hour response SLA for inbound interest (recruiters, customers, collaborators — whatever "inbound" means for the goal).
9. Day-60 review answered from tracker evidence.

### 6. Hand off

End by outputting: the generated file list, the user's first-7-days checklist (from their PLAN.md), and the instruction to run `/weekly-reset` every Sunday. Remind them: fill any CUSTOMIZE blocks in generated skills, and make the fork private (or enable the `.gitignore` privacy section) before committing personal data.

## Rules

- Never generate a system for a goal you haven't sharpened into something measurable at Day 60.
- Specificity is the whole game: a generated skill that says "search for opportunities online" is a failure; name the boards, APIs, communities, or measurement protocols for the domain. If you don't know them, research before generating.
- Do not exceed 5 pipeline skills — more stages than a weekly cycle can run is process theater.
- Generated skills go in `.claude/skills/` (live), never in `examples/` (reference).
- If the goal is job-search-shaped, adapt `examples/job-search/skills/` directly (copy → fill CUSTOMIZE blocks from the interview) instead of generating from scratch.
- The framework files (`framework/`, `PLAN_TEMPLATE.md`, this skill, `/weekly-reset`) are never modified by generation.
