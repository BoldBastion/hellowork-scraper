[Hellowork Scraper](https://apify.com/unfenced-group/hellowork-scraper?fpr=data)

# HelloWork Scraper

![HelloWork Scraper](https://images.apifyusercontent.com/PdUJONWfMUba7pJX3beKMwr1y_sYYGPW_2BUUiKOKJM/w:1800/cb:1/aHR0cHM6Ly9pLmltZ3VyLmNvbS9IVWVCSTJYLnBuZw.webp)

Scrape structured job listings from [HelloWork](https://www.hellowork.com) — France. 50,000+ active listings. No API key required.

---

## Why this scraper?

### 🌍 HelloWork — France's job market

Scrape structured job data from HelloWork, one of the leading employment platforms in France.

### 📄 Full job descriptions

Enable `fetchDetails` to retrieve complete job descriptions in HTML, plain text, and Markdown format.

### 💰 Structured salary data

Salary ranges extracted and normalised into structured `salaryMin`, `salaryMax`, `salaryCurrency`, and `salaryPeriod` fields where published.

### 🔄 Repost detection

Cross-run deduplication with a 90-day TTL. Use `skipReposts: true` to receive only genuinely new listings.

### 🔗 Direct URL scraping

Supply specific HelloWork search or category URLs via `startUrls` for precise targeting.

### ⚙️ No API key required

Runs without any third-party credentials.

---

## Input parameters

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `keywords` | string | Job title, keyword, or skill to search for. | — |
| `location` | string | City or region to filter by. Leave empty to search all locations. | — |
| `contractType` | string | Contract type filter (e.g. permanent, contract, temporary). | — |
| `maxResults` | integer | Maximum number of results to return. | `5` |
| `daysOld` | integer | Only return listings published within the last N days. | `0` |
| `skipReposts` | boolean | Skip listings already seen in previous runs (90-day deduplication window). | `false` |
| `startUrls` | array | List of specific URLs to scrape. Bypasses the search input. | — |

---

## Output schema

Each result contains the following fields.

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Unique job listing ID from the source platform. |
| `url` | string | Direct URL to the job listing. |
| `title` | string | Job title as published. |
| `company` | string | Employer / company name. |
| `location` | string | Full location string as published. |
| `city` | string | City of the work location. |
| `country` | string | Country code (ISO 3166-1 alpha-2). |
| `contractType` | string | Contract type (permanent, contract, temporary, etc.). |
| `workSchedule` | string | Work schedule (full-time, part-time, etc.). |
| `salaryMin` | number | Minimum salary (null if not published by employer). |
| `salaryMax` | number | Maximum salary (null if not published by employer). |
| `salaryCurrency` | string | ISO 4217 currency code (null if no salary published). |
| `salaryPeriod` | string | Salary period: YEAR / MONTH / WEEK / DAY / HOUR. |
| `publishDate` | string | Publication date (YYYY-MM-DD). |
| `publishDateISO` | string | Publication date in ISO 8601 format. |
| `source` | string | Source domain name. |
| `scrapedAt` | string | ISO 8601 timestamp of when this item was scraped. |
| `contentHash` | string | MD5 hash of key fields for change detection (16 chars). |
| `summary` | string | Human-readable one-line summary of the listing. |
| `changeStatus` | string | Change status: NEW / MODIFIED / UNCHANGED. |
| `isRepost` | boolean | True if this listing was seen in a previous run (90-day window). |
| `originalPublishDate` | string | Original publish date if this is a repost (null otherwise). |
| `originalUrl` | string | Original URL if this is a repost (null otherwise). |

**Example output record:**

```
{
  "id": "123456",
  "url": "https://www.hellowork.com/jobs/senior-developer/123456",
  "title": "Commercial B2B",
  "company": "Société Générale",
  "location": "Lyon",
  "city": "Lyon",
  "country": "FR",
  "contractType": "Permanent",
  "workSchedule": "Full-time",
  "salaryMin": 45000,
  "salaryMax": 60750,
  "salaryCurrency": "EUR",
  "salaryPeriod": "YEAR",
  "publishDate": "2026-04-15",
  "publishDateISO": "2026-04-15",
  "source": "hellowork.com",
  "scrapedAt": "2026-04-24T09:00:00.000Z",
  "contentHash": "a3f1b2c4d5e67890",
  "summary": "Commercial B2B · Société Générale · Lyon",
  "changeStatus": "NEW",
  "isRepost": false,
  "originalPublishDate": null,
  "originalUrl": null
}
```

---

## Examples

### 1 — Search for Commercial B2B roles in Lyon

```
{
  "searchQuery": "commercial",
  "location": "Lyon",
  "maxResults": 100
}
```

### 2 — Filter by contract type — permanent positions only

```
{
  "searchQuery": "",
  "contractType": "permanent",
  "maxResults": 200
}
```

### 3 — Scrape a specific search page directly via startUrls

```
{
  "startUrls": [
    {
      "url": "https://www.hellowork.com/jobs?q=commercial"
    }
  ],
  "maxResults": 50
}
```

### 4 — Daily feed — new listings only, past 24 hours, no reposts

```
{
  "searchQuery": "",
  "daysOld": 1,
  "skipReposts": true,
  "maxResults": 1000
}
```

---

## 💰 Pricing

**$1.50 per 1,000 results** — you only pay for successfully retrieved listings.
Failed retries and filtered reposts are never charged.

| Results | Cost |
| --- | --- |
| 100 | ~$0.15 |
| 1,000 | ~$1.50 |
| 10,000 | ~$15.00 |
| 100,000 | ~$150.00 |

> Flat-rate alternatives typically charge $29–$49/month regardless of usage.

Use the **Max results** cap in the input to control your spend exactly.

---

## Performance

| Run size | Approx. time |
| --- | --- |
| 100 listings | ~2 min |
| 1,000 listings | ~15 min |
| 10,000 listings | ~2.5 hours |

---

## Known limitations

- **Salary:** Not all employers publish salary information — `salaryMin` and `salaryMax` may be `null`.
- **fetchDetails:** Setting `fetchDetails: false` returns list-page fields only; description fields will be `null`.

---

## Technical details

- **Source:** hellowork.com — France's job market
- **Memory:** 256 MB
- **Repost storage:** KeyValueStore `hellowork-job-dedup`, 90-day TTL
- **Retry:** Automatic retry on network errors, exponential backoff, 3 attempts per request

---

## Additional services

Need a custom actor, additional filters, scheduled runs, or integration support?
Send an email to [info@unfencedgroup.nl](mailto:info@unfencedgroup.nl) — we build on request.

---

*Part of the [Unfenced Group](https://apify.com/unfenced-group) European job board scraper portfolio — 50+ job markets covered.*
*Built by [unfenced-group](https://apify.com/unfenced-group) · Issues? Open a ticket or send a message.*