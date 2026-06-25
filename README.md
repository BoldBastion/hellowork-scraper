[Hellowork Scraper](https://apify.com/blackfalcondata/hellowork-scraper?fpr=data)

## What does Hellowork Jobs Scraper do?

Hellowork Jobs Scraper extracts structured job data from [hellowork.com](https://hellowork.com) — including salary data, contact details, company metadata, full descriptions, and location data. It supports keyword search, location filters, and controllable result limits, so you can run the same query consistently over time. The actor also offers detail enrichment (full descriptions, company profiles, and contact information) where the source provides them.

**New to Apify?** [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) and use the included $5 monthly platform credit to test this actor.

## Key features

- **Incremental mode** — recurring runs emit and charge only for listings that are new or whose tracked content changed. First run builds the baseline state; subsequent runs emit only new or changed records.
- **Detail enrichment** — full descriptions, company profiles, and contact information where the source provides them.
- **Compact mode** — AI-agent and MCP-friendly payloads with core fields only.

## What data can you extract from hellowork.com?

Each result includes Core listing fields (`jobId`, `jobKey`, `title`, `location`, `locationRegion`, `locationPostalCode`, `locationCountry`, and `salaryText`, and more), detail fields when enrichment is enabled (`description`, `descriptionText`, `descriptionHtml`, `descriptionMarkdown`, and `descriptionLength`), contact and apply information (`directApply`, `applyUrl`, and `extractedEmails`), and company metadata (`company`, `companyUrl`, and `companyLogo`). In standard mode, all fields are always present — unavailable data points are returned as `null`, never omitted. In compact mode, only core fields are returned.

Enable detail enrichment in the input to get richer fields such as full descriptions, company profiles, and contact information where the source provides them.

## Input

The main inputs are a search keyword, an optional location filter, and a result limit. Additional filters and options are available in the input schema.

Key parameters:

- **`query`** — Job search keywords. Use JSON array for multi-query.
- **`country`** — Which hellowork.com market to search. (default: `"FR"`)
- **`location`** — City, state, or region. Use JSON array for multi-location.
- **`startUrls`** — Direct search or job detail URLs.
- **`maxResults`** — Maximum total job listings to return (0 = unlimited). (default: `25`)
- **`maxPages`** — Maximum SERP pages to scrape per search source. (default: `5`)
- **`daysPosted`** — Filter jobs by recency. Use '1' for incremental daily runs (only jobs posted in last 24h). (default: `"all"`)
- **`includeDetails`** — Fetch each job's detail page for full description, salary data, and company info. (default: `true`)
- **`includeCompanyProfile`** — Fetch company overview pages for industry, headcount, and more. (default: `false`)
- **`descriptionMaxLength`** — Truncate description to this many characters. 0 = no truncation. (default: `0`)
- **`compact`** — Output only core fields (for AI-agent/MCP workflows). (default: `false`)
- **`incrementalMode`** — Compare against previous run state. Requires stateKey. (default: `false`)
- ...and 4 more parameters

### Input examples

**Basic search** — Keyword-driven search with a result cap.

→ Full payload per result — all standard fields populated where the source provides them.

```
{
  "query": "software engineer",
  "maxResults": 50
}
```

**Incremental tracking** — Only emit jobs that changed since the previous run with this `stateKey`.

→ First run builds the baseline state. Subsequent runs emit only records that are new or whose tracked content changed. Set `emitUnchanged: true` to include unchanged records as well.

```
{
  "query": "software engineer",
  "maxResults": 200,
  "incrementalMode": true,
  "stateKey": "software-engineer-tracker"
}
```

**Compact output for AI agents** — Return only core fields for AI-agent and MCP workflows.

→ Small payload with the most important fields — ideal for piping into LLMs without token overhead.

```
{
  "query": "software engineer",
  "maxResults": 50,
  "compact": true
}
```

## Output

Each run produces a dataset of structured job records. Results can be downloaded as JSON, CSV, or Excel from the Dataset tab in Apify Console.

### Example job record

```
{
  "jobId": "10676b1b4dadad8cb24dbeab3ac5635a1ac3cce27c694a0bbb758fc2b4afbf0a",
  "jobKey": "75605607",
  "title": "Developpeur H/F",
  "company": "W Hunt",
  "companyUrl": "https://www.hellowork.com/fr-fr/entreprises/w-hunt-154580.html",
  "companyLogo": "https://f.hellowork.com/img/entreprises/160_160/154580.png",
  "location": "Paris",
  "locationRegion": "Île-de-France",
  "locationPostalCode": "75000",
  "locationCountry": "FR",
  "description": "Détail du posteW Hunt, marque de W Executive, recherche pour son client, un Analyste Programmeur/Développeur.NET F/H.Il s'agit d'un poste à pourvoir en CDI, situé en région Parisienne / Contexte inter...",
  "descriptionText": "Détail du posteW Hunt, marque de W Executive, recherche pour son client, un Analyste Programmeur/Développeur.NET F/H.Il s'agit d'un poste à pourvoir en CDI, situé en région Parisienne / Contexte inter...",
  "descriptionHtml": "<h2>Détail du poste</h2><p><br />W Hunt, marque de W Executive, recherche pour son client, un Analyste Programmeur/Développeur.NET F/H.<br /><br />Il s'agit d'un poste à pourvoir en CDI, situé en régi...",
  "descriptionMarkdown": "## Détail du poste\n\nW Hunt, marque de W Executive, recherche pour son client, un Analyste Programmeur/Développeur.NET F/H.\n\nIl s'agit d'un poste à pourvoir en CDI, situé en région Parisienne / Context...",
  "descriptionLength": 2594,
  "salaryText": "50 000 - 55 000 EUR / YEAR",
  "salaryMin": 50000,
  "salaryMax": 55000,
  "salaryCurrency": "EUR",
  "salaryUnit": "YEAR",
  "contractType": "CDI",
  "employmentType": "FULL_TIME",
  "industry": "Services aux Entreprises",
  "occupationalCategory": "Informatique",
  "educationLevel": "postgraduate degree",
  "experienceMonths": 12,
  "directApply": true,
  "publishedAge": "il y a 6 jours",
  "postedAt": "2026-04-13T00:06:43Z",
  "validThrough": "2026-05-13T00:06:43Z",
  "canonicalUrl": "https://www.hellowork.com/fr-fr/emplois/75605607.html",
  "applyUrl": null,
  "sourceUrl": "https://www.hellowork.com/fr-fr/emplois/75605607.html",
  "sourceDomain": "www.hellowork.com",
  "searchQuery": "developpeur",
  "searchUrl": "https://www.hellowork.com/fr-fr/emploi/recherche.html?k=developpeur",
  "isSponsored": false,
  "fetchedAt": "2026-04-19T21:36:17.607Z",
  "detailFetched": true,
  "contentQuality": "full",
  "extractedEmails": [],
  "isRepost": null,
  "repostOfId": null,
  "repostDetectedAt": null,
  "changeType": "NEW",
  "trackedHash": "ac5bd7389d6495700ead83edb30d1b36841227a67171bbb48e58bd3971eaee34",
  "firstSeenAt": "2026-04-19T21:36:17.607Z",
  "lastSeenAt": "2026-04-19T21:36:17.607Z",
  "previousSeenAt": null,
  "expiredAt": null,
  "stateKey": "live-verify-1776634572768"
}
```

### Incremental fields

When `incremental: true`, each record also carries:

- `changeType` — one of `NEW`, `UPDATED`, `UNCHANGED`, `REAPPEARED`, `EXPIRED`. Default output covers `NEW` / `UPDATED` / `REAPPEARED`; set `emitUnchanged: true` or `emitExpired: true` to opt into the others.
- `firstSeenAt`, `lastSeenAt` — ISO-8601 timestamps tracking the listing across runs.
- `isRepost`, `repostOfId`, `repostDetectedAt` — populated when a new listing matches the tracked content of a previously expired one. Set `skipReposts: true` to drop detected reposts from the output.

## How to scrape hellowork.com

1. Go to [Hellowork Jobs Scraper](https://apify.com/blackfalcondata/hellowork-scraper?fpr=1h3gvi) in Apify Console.
2. Enter a search keyword and optional location filter.
3. Set `maxResults` to control how many results you need.
4. Enable `includeDetails` if you need full descriptions, contact info, or company data.
5. Click **Start** and wait for the run to finish.
6. Export the dataset as JSON, CSV, or Excel.

## Use cases

- Extract job data from hellowork.com for market research and competitive analysis.
- Track salary trends across regions and categories over time.
- Monitor new and changed listings on scheduled runs without processing the full dataset every time.
- Build outreach lists using contact details and apply URLs from listings.
- Research company hiring patterns, employer profiles, and industry distribution.
- Use structured location data for regional analysis, mapping, and geo-targeting.
- Feed structured data into AI agents, MCP tools, and automated pipelines using compact mode.
- Export clean, structured data to dashboards, spreadsheets, or data warehouses.

## How much does it cost to scrape hellowork.com?

Hellowork Jobs Scraper uses [pay-per-event](https://docs.apify.com/platform/actors/paid-actors/pay-per-event) pricing. You pay a small fee when the run starts and then for each result that is actually produced.

- **Run start:** $0.008 per run
- **Per result:** $0.001 per job record

Example costs:

- 10 results: **$0.02**
- 100 results: **$0.11**
- 500 results: **$0.51**

### Example: recurring monitoring savings

These examples compare full re-scrapes with incremental runs at different churn rates. Churn is the share of listings that are new or whose tracked content changed since the previous run. Actual churn depends on your query breadth, source activity, and polling frequency — the scenarios below are examples, not predictions.

Example setup: 100 results per run, daily polling (30 runs/month). Event-pricing examples scale linearly with result count.

| Churn rate | Full re-scrape run cost | Incremental run cost | Savings vs full re-scrape | Monthly cost after baseline |
| --- | --- | --- | --- | --- |
| 5% — stable niche query | $0.11 | $0.01 | $0.10 (88%) | $0.39 |
| 15% — moderate broad query | $0.11 | $0.02 | $0.09 (79%) | $0.69 |
| 30% — high-volume aggregator | $0.11 | $0.04 | $0.07 (65%) | $1.14 |

Full re-scrape monthly cost at daily polling: $3.24. First month with incremental costs $0.49 / $0.78 / $1.21 for the 5% / 15% / 30% scenarios because the first run builds baseline state at full cost before incremental savings apply.

Platform usage (compute and proxies) is billed separately by Apify based on actual consumption. Incremental runs consume less on result processing, though fixed per-run overhead stays the same.

 

## FAQ

### How many results can I get from hellowork.com?

The number of results depends on the search query and available listings on hellowork.com. Use the `maxResults` parameter to control how many results are returned per run.

### Does Hellowork Jobs Scraper support recurring monitoring?

Yes. Enable incremental mode to only receive new or changed listings on subsequent runs. This is ideal for scheduled monitoring where you want to track changes over time without re-processing the full dataset.

### Can I integrate Hellowork Jobs Scraper with other apps?

Yes. Hellowork Jobs Scraper works with Apify's [integrations](https://apify.com/integrations?fpr=1h3gvi) to connect with tools like Zapier, Make, Google Sheets, Slack, and more. You can also use webhooks to trigger actions when a run completes.

### Can I use Hellowork Jobs Scraper with the Apify API?

Yes. You can start runs, manage inputs, and retrieve results programmatically through the [Apify API](https://docs.apify.com/api/v2). Client libraries are available for JavaScript, Python, and other languages.

### Can I use Hellowork Jobs Scraper through an MCP Server?

Yes. Apify provides an [MCP Server](https://apify.com/apify/actors-mcp-server?fpr=1h3gvi) that lets AI assistants and agents call this actor directly. Use compact mode and `descriptionMaxLength` to keep payloads manageable for LLM context windows.

### Is it legal to scrape hellowork.com?

This actor extracts publicly available data from hellowork.com. Web scraping of public information is generally considered legal, but you should always review the target site's terms of service and ensure your use case complies with applicable laws and regulations, including GDPR where relevant.

### Your feedback

If you have questions, need a feature, or found a bug, please [open an issue](https://apify.com/blackfalcondata/hellowork-scraper/issues?fpr=1h3gvi) on the actor's page in Apify Console. Your feedback helps us improve.

## You might also like

- [Actiris Brussels Job Scraper](https://apify.com/blackfalcondata/actiris-scraper?fpr=1h3gvi) — Scrape all active job listings from actiris.brussels — official Brussels public employment service..
- [Adzuna Job Scraper](https://apify.com/blackfalcondata/adzuna-scraper?fpr=1h3gvi) — Scrape adzuna.com - the global job board with 20+ country markets. Structured salary.
- [APEC.fr Scraper - French Executive Jobs](https://apify.com/blackfalcondata/apec-scraper?fpr=1h3gvi) — Scrape apec.fr - French executive job listings with salary ranges, company, location, skills,.
- [Arbeitsagentur Scraper - German Jobs](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Scrape arbeitsagentur.de - Germany’s official employment portal with 1M+ listings. Contact data,.
- [AutoScout24 Scraper](https://apify.com/blackfalcondata/autoscout24-scraper?fpr=1h3gvi) — Scrape autoscout24.com - Europe's largest used car marketplace with 770K+ listings. Structured.
- [Bayt.com Scraper - Jobs from the Middle East](https://apify.com/blackfalcondata/bayt-scraper?fpr=1h3gvi) — Scrape bayt.com - the leading Middle East job board. Salary data, experience requirements.
- [Bilbasen Scraper - Denmark’s Car Marketplace](https://apify.com/blackfalcondata/bilbasen-scraper?fpr=1h3gvi) — Scrape bilbasen.dk - Denmark’s largest car marketplace. Full vehicle specifications, seller.
- [Bumeran Scraper](https://apify.com/blackfalcondata/bumeran-scraper?fpr=1h3gvi) — Scrape bumeran.com.ar - the largest job board across 8 LATAM countries. Work modality, contract.

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open this actor and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).