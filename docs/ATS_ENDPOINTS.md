# Polling ATS Job Boards Directly

The highest-leverage trick in this system: **most companies' career sites are thin frontends over a handful of Applicant Tracking Systems (ATS), and most of those expose public, unauthenticated JSON endpoints.** Polling them directly is fresher, cheaper, and more complete than scraping aggregators — and posting dates are accurate (aggregators routinely serve reposts and stale listings).

Resolve each target company's endpoint **once**, cache it in `pipeline/target-companies.md`, and your weekly scan becomes a short loop of HTTP calls.

## How to identify a company's ATS

Open the company's careers page and look at the URL (or the network tab):

| URL contains | ATS |
|---|---|
| `myworkdayjobs.com` or `wd1`/`wd5`/`wd12`… | Workday |
| `boards.greenhouse.io` / `greenhouse.io` | Greenhouse |
| `jobs.lever.co` | Lever |
| `careers.` + `successfactors` / `rmk` / `sap` markers | SAP SuccessFactors |
| `recruiting.ultipro.com` / `ukg` | UKG / UltiPro |
| `icims.com` | iCIMS |
| none of the above | Custom portal (scrape or use their search URL) |

A web search for `"{company}" careers workday` (or greenhouse/lever) usually reveals it in one shot.

## Workday — the big one

Huge share of F500 employers. The career site at `https://{company}.wd{N}.myworkdayjobs.com/{site}` is backed by a JSON API:

```bash
curl -s -X POST \
  "https://{host}/wday/cxs/{tenant}/{site}/jobs" \
  -H "Content-Type: application/json" \
  -d '{"appliedFacets":{},"limit":20,"offset":0,"searchText":"financial analyst"}'
```

- `{host}` — e.g. `examplecorp.wd5.myworkdayjobs.com`
- `{tenant}` — usually the subdomain (`examplecorp`)
- `{site}` — the board name from the career-site URL path (`External`, `Careers`, etc.)

**Response fields that matter:**
- `jobPostings[].title`
- `jobPostings[].postedOn` — human-readable ("Posted 3 Days Ago"); use it for the ≤30-day filter
- `jobPostings[].externalPath` — append to `https://{host}/{site}` for the apply URL
- `total` — paginate with `offset` in steps of `limit`

No auth needed for public boards. Be polite: a handful of requests per company per week, not a crawler.

## Greenhouse

```bash
curl -s "https://boards-api.greenhouse.io/v1/boards/{board_token}/jobs?content=true"
```

The `board_token` is in the careers URL (`boards.greenhouse.io/{token}`). Returns every open role with `updated_at`, `location`, `absolute_url`, and full HTML content when `content=true`. Filter client-side.

## Lever

```bash
curl -s "https://api.lever.co/v0/postings/{company_token}?mode=json"
```

Token from `jobs.lever.co/{token}`. Returns `createdAt` (epoch ms — best posting-date signal), `categories`, `hostedUrl`, and full description text.

## SAP SuccessFactors

Less uniform. Most boards accept a search URL with query params — capture the URL your browser produces after a keyword search and fetch it directly. Some tenants expose `/careers/api/` JSON routes visible in the network tab.

## UKG / UltiPro

Board at `recruiting.ultipro.com/{TENANT}/JobBoard/{guid}/`. The `JobBoardView/LoadSearchResults` POST endpoint (visible in the network tab) returns JSON. Capture it once from your browser's dev tools and replay with `curl`.

## iCIMS

Boards at `{tenant}.icims.com/jobs/search`. Server-rendered but predictable: paginate with `&pr={page}` and parse; posting dates are in each job's detail page.

## Custom portals

Big retailers and tech companies often run their own (e.g. `careers.{company}.com`). Two options:
1. **Network tab first** — most custom frontends still call an internal JSON search API you can replay.
2. Otherwise scrape the search-results page with your fetch tool of choice.

## The caching discipline

The whole point is *resolve once, poll forever*:

1. During a scan, when you hit a company with no cached endpoint (the backlog tier), spend the 2 minutes to resolve it.
2. Write the working endpoint + any request body **back into `pipeline/target-companies.md`** immediately.
3. Next week's scan reads the cache and never re-derives it.

## Etiquette and reliability

- These are public, unauthenticated endpoints serving the same data as the careers page — but respect them: low volume, weekly cadence, no parallel hammering.
- Endpoints occasionally move when a company migrates ATS. When one 404s, re-resolve and update the cache.
- Always read the board's own posting date. Never trust an aggregator's "posted" field.
- Salary is often unlisted. Record blank — never fabricate.
