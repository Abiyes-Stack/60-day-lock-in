---
name: application-pack
description: Use when the user is ready to apply to a specific role or asks for tailored application materials — resume bullets, summary, cover points, referral or networking message, follow-up plan.
---

# Application Pack

One role → tailored materials + referral message + follow-up date. Tracker: `pipeline/tracker.csv`.

<!-- CUSTOMIZE ─────────────────────────────────────────────
- IDENTITY: the user's one-sentence market identity (from the plan)
- BACKGROUND: their real, provable experience — employers, projects,
  education, tools. Every bullet must map to something here.
- WARM POOLS: their contact pools (former colleagues, alumni,
  community, target-company connections) and their strongest
  referral network to route first.
- RESUME SOURCE: local resume file path, or a connector that can
  fetch it.
──────────────────────────────────────────────────────────── -->

## Recipe

1. Gather inputs: the posting (tracker row or URL — pull the full JD), the matching cluster file in `pipeline/clusters/`, the most relevant project link, and the user's background (see CUSTOMIZE block / local resume).
2. Produce the pack with exactly these sections (template: `pipeline/packs/_TEMPLATE.md`):
   - **Tailored summary** — 2–3 sentences leading with the user's market identity. Never "aspiring". Match emphasis to the role's lane; keep every statement provable.
   - **3–5 resume bullets** — mirror the cluster's verbatim keywords, but every claim must map to real experience from the background block.
   - **Project link** — one line on why it's relevant to this role's problems.
   - **Warm outreach** — under 120 words per message: names the role, one specific hook from the project or shared background, a small clear ask (insight, a 10-minute conversation, a resume referral, or "who's the best person to contact"). Identify one or two people with a *credible* connection to the company, team, or role — never force two messages when only one relationship is real. Route the strongest referral network first. Use LinkedIn to find the person, not to fetch jobs. For roles scoring 8–10, send the outreach before or immediately after submission.
   - **Follow-up date** — 5–7 business days after applying.
3. Save to `pipeline/packs/YYYY-MM-DD-<company>-<slug>.md`.
4. Update the tracker row: status (`applying`, then `applied` when the user confirms), contact_or_referral, applied_date, follow_up_date, project_link — plus outreach date sent, reply, and next action in notes.

## Rules

- Mirror JD language, never JD claims — if the user can't prove it in an interview, it doesn't go in a bullet.
- A name beats a title, a title beats "the team".
- If no project matches the role yet, say so and link the closest one rather than overstating fit.
- Recruiters, referrals, and interview invitations get a response within 24 hours — surface any pending ones whenever this skill runs.
