---
name: weekly-reset
description: Use when the user asks for their Sunday reset, weekly review, week planning, mode check, or asks how the lock-in is going. Works with any generated lock-in system.
---

# Weekly Reset

The 30–45 minute Sunday ritual, automated. Reads `PLAN.md` and `pipeline/tracker.csv` (whatever schema `/lock-in-setup` generated), reports honestly, and sets up the coming week.

## Recipe

1. Read `PLAN.md` (the plan's mode table, sprint dates, and floors) and the full tracker.
2. **Report the week that just ended, against the mode that was chosen:**
   - Each mode metric: floor vs. actual (from tracker rows dated this week).
   - Overdue items: follow-ups past their date, inbound interest waiting >24h — these go first, flagged loudly.
   - Sprint position: which day of which sprint, on-track/at-risk/slipped, in one line each.
3. **Evidence check:** what did this week's data points say? (New scored opportunities, measurements, or market signals — and whether any plan assumption now looks wrong. During week 1, track progress toward the ≥20-point evidence goal.)
4. **Recommend next week's mode** (Recovery / Core / Target / Stretch) from: actuals vs. floors, calendar load the user confirms, and sprint position. Recommend, then ask — mode choice is the user's.
5. **Pick the week's anchor:** the single highest-leverage item (next opportunity to pursue, next increment to ship, next assessment gate). One anchor, not three.
6. Write the reset into the tracker notes or a `pipeline/resets/YYYY-MM-DD.md` line-item summary, and output: overdue actions → mode → anchor → the 3–5 concrete next actions with their calendar blocks.

## Rules

- Report actuals honestly — a missed floor is stated as missed, never reframed. The recovery protocol (in the plan) handles it without shame: smallest momentum-restoring action first.
- Never roll every missed task forward; return to the next themed block.
- If two consecutive weeks missed Core floors, recommend Recovery mode and say why — pushing Target after two missed weeks is how sprints die.
- On or after the plan's end date, run the Day-60 review section of the plan from tracker evidence instead of a normal reset.
