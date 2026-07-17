# System Spec — The Lock-In Meta-System

> **Purpose:** a self-contained, agent-portable specification for AI tools other than Claude Code (Codex, Cursor, raw LLMs). It covers both jobs an agent performs in this system: **(A) generating** a goal-specific lock-in from a user's goal + context, and **(B) operating** a generated system week to week. The framework rules live in `framework/OPERATING_SYSTEM.md`; read it first — this spec assumes it.

## A. The generation contract

Input: one goal + interview answers. Output: a personalized system. This is what `/lock-in-setup` does; any agent can follow the same contract.

### A.1 Interview (keep it to ~2 rounds)

Collect: start date and duration (default 60 days; 30/90 allowed) · honest hours/week and which hours · fixed commitments to protect (income source, family, recurring events) · current assets (skills, work, audience, network, equipment) · the win condition **plus a fallback outcome the user fully controls** · parallel obligations with deadlines · health non-negotiables.

Sharpen vague goals into one Day-60-measurable sentence before generating. State inferred assumptions in the generated plan.

### A.2 Classify the archetype

- **Conversion** — success = someone says yes (job, clients, funding, admissions, bookings). Loop: scout → decode → build proof → convert + warm outreach → follow up.
- **Launch** — success = a thing ships to a real audience (product, book, channel). Loop: define audience/offer → build increments → ship gates → distribute → measure.
- **Progress** — success = a measured change in the user (fitness, language, exam). Loop: baseline → practice → re-assessment gates → adjust → final assessment.

Blends are normal; one archetype leads.

### A.3 Generate these files

1. **`PLAN.md`** (repo root) — fill `PLAN_TEMPLATE.md` completely; zero placeholders survive; sprint dates computed; daily blocks mapped to the user's real hours; parallel tracks bounded or deleted.
2. **3–5 pipeline skills** — one per loop stage, in `.claude/skills/<name>/SKILL.md` (or your tool's equivalent). Each must contain: a domain-specific kebab-case name; trigger description ("Use when the user…"); inputs/outputs with exact repo-relative paths; a numbered executable Recipe naming **real** data sources, endpoints, communities, or measurement protocols for the domain (research them if unknown — "search online" is a failed generation); a Rules section with honesty constraints, freshness windows, quality floors, and skip conditions; a handoff line to the next stage.
3. **`pipeline/tracker.csv`** — headers + one clearly-fictional example row. Unit of work = the goal's atom (conversion: opportunity; launch: shippable increment; progress: assessment/session). Required columns: date, status (flow defined in pipeline/README), 0–10 score + per-criterion breakdown, evidence/link, next-action date.
4. **`pipeline/README.md`** — weekly flow, which skill runs when, status flow, column reference.
5. **`pipeline/**/_TEMPLATE.md`** — one per artifact type the skills produce.

Reference implementation of this contract: `examples/job-search/` (conversion archetype). Match its specificity.

### A.4 Universal invariants (survive into every generated system)

1. Tracker = single source of truth.
2. Evidence week: ≥20 scored real-world data points in days 1–7; later invest decisions trace to them.
3. 0–10 rubric, five 0–2 criteria; act 8–10, selective 6–7, skip <6.
4. Three ~2-week build sprints + conversion window; overrun → cut scope, ship smallest credible version.
5. Acceptance standard per artifact: one question; sources/dates/licenses; personally-validated core; honest labeling (simulations labeled; no fabricated metrics/testimonials); distribution kit (link, summary, 60-second walkthrough, two stories).
6. Modes Recovery/Core/Target/Stretch as outcome floors, chosen weekly; Stretch ≤2 consecutive weeks.
7. Recovery protocol: never roll everything forward; smallest momentum-restoring action; protect sleep/income/responsiveness.
8. 24-hour SLA on inbound interest.
9. Final review answered from tracker evidence only.

### A.5 Generation guardrails

- ≤5 pipeline skills — more stages than a week can run is process theater.
- Generated files go to live paths; never modify `framework/`, `PLAN_TEMPLATE.md`, the generator, or `examples/`.
- Job-search-shaped goals: adapt `examples/job-search/skills/` (copy + fill CUSTOMIZE blocks) rather than regenerate.
- Remind the user about fork privacy before they commit personal data.

## B. The operating contract (weekly)

### B.1 During the week

Run the generated pipeline skills as their triggers arise. Always: read the tracker before adding to it (no duplicates; surface overdue follow-ups first); verify dates/claims from primary sources; never fabricate numbers — blank + a note beats a guess; keep links intact; respond to inbound within 24 hours.

### B.2 The Sunday reset (`/weekly-reset` equivalent)

1. Read `PLAN.md` (modes, floors, sprint dates) + full tracker.
2. Report actuals vs. the chosen mode's floors, honestly; a missed floor is "missed."
3. Flag overdue follow-ups and >24h inbound first, loudly.
4. State sprint position (day N of sprint M; on-track/at-risk/slipped).
5. Evidence check: what did this week's data points say; is any plan assumption now wrong?
6. Recommend next week's mode (two consecutive missed-Core weeks → recommend Recovery, and say why); the user chooses.
7. Pick ONE anchor for the week.
8. Output: overdue actions → mode → anchor → 3–5 next actions mapped to calendar blocks.

### B.3 End of the run

On/after the plan's end date, replace the reset with the plan's final review — every answer cited from tracker rows. The next 30-day lane is chosen from that evidence.

## C. Tooling portability

| Function | Minimum viable tool |
|---|---|
| Fetching/scouting | HTTP client (`curl`); domain APIs per generated skills |
| Tracker | Any CSV editor |
| Skills | Markdown checklists a human can run manually |
| People research | Manual LinkedIn / community search (never scraped) |
| Outreach & follow-ups | Any mail/messaging client + the 24h SLA |

The system degrades gracefully: with no AI at all, the generated SKILL.md files are runnable checklists.
