# Target Companies

> Your curated employer list + cached ATS endpoints. The weekly scan polls these boards FIRST — they're fresher and more complete than any aggregator. Resolve each company's endpoint once (guide: `../docs/ATS_ENDPOINTS.md`), cache it here, re-poll cheaply forever.
>
> Replace the example entries with your own companies. Tier by referral strength, not prestige.

## Tier 1 — {{YOUR_METRO}} anchors (strongest referral network)

| Company | ATS | Endpoint / board | Notes |
|---|---|---|---|
| {{Company A}} | Workday | `POST https://{host}.wd5.myworkdayjobs.com/wday/cxs/{tenant}/{site}/jobs` | {{referral contact pool}} |
| {{Company B}} | Greenhouse | `GET https://boards-api.greenhouse.io/v1/boards/{token}/jobs?content=true` | |
| {{Company C}} | Custom portal | `{{careers URL}}` | needs scraping |

## Tier 2 — target metros and sectors

| Company | ATS | Endpoint / board | Notes |
|---|---|---|---|
| {{Company D}} | Lever | `GET https://api.lever.co/v0/postings/{token}?mode=json` | |
| {{Company E}} | SuccessFactors | `{{careers URL with keyword params}}` | |
| {{Company F}} | UKG/UltiPro | `{{recruiting URL}}` | |

## Backlog — resolve on first scan

> Companies you haven't resolved an endpoint for yet. During a scan, search "`{company}` careers workday" (or greenhouse/lever), capture the board URL, and move the row up into a tier with its endpoint cached.

- {{Company G}}
- {{Company H}}
- {{Company I}}

## Scan order

1. Tier 1 first (referral leverage), then Tier 2, then backlog as time allows.
2. Keyword sets come from your plan's lanes.
3. Keep only postings ≤30 days old — read the board's own posted date, never the aggregator's.
