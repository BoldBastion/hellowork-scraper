[Hellowork Scraper](https://apify.com/dltik/hellowork-scraper?fpr=data)

# HelloWork Scraper — French Job Board API (800K+ Active Offers)

> Scrape **HelloWork.com**, France's #1 private job board with 800K+ active offers. Filter by keyword, city, contract type, salary range, remote policy. Get title, company, location, contract (CDI/CDD/alternance/stage), salary, remote, full description, posted date. HTTP-only, **$0.001 per job** ($1 per 1,000).

⭐ **Bookmark this HelloWork Scraper** — Apify ranks actors by bookmarks, so it directly helps the visibility of this scraper on the Apify Store.

## What is the HelloWork Scraper?

The **HelloWork Scraper** is an Apify actor that pulls live job offers from [HelloWork](https://www.hellowork.com/) — France's largest private job board, generally complementary to France Travail (which is public). HelloWork hosts 800K+ active offers across all sectors, contract types, and seniority levels. This scraper accepts complex filters (keyword, city, contract, salary, remote) and returns structured JSON with title, company, full description, salary range, posted date, application URL.

No HelloWork account, no OAuth, no quota. Pure HTTP scraping with stealth headers.

## Use cases

- **ATS feeders / job aggregators** — auto-import filtered HelloWork offers into your platform daily.
- **Salary benchmarking** — aggregate `data engineer Paris CDI` salaries across HelloWork to inform your comp band.
- **Recruitment intelligence** — track who's posting which roles in your competitive landscape.
- **Niche job board** — build a sector-specific job board (e.g., construction jobs in Auvergne) on top of HelloWork.
- **Career-services bots** — daily Telegram alert with new HelloWork jobs matching a candidate profile.
- **Labor-market analytics** — count open positions per region per sector over time.

## Input

```
{
  "keywords": "developer python",
  "city": "Paris",
  "contractTypes": ["CDI", "Freelance"],
  "salaryMin": 45000,
  "remote": "remote_friendly",
  "limit": 200
}
```

## Output

```
{
  "id": "HW-EXAMPLE-12345",
  "title": "Développeur Python Senior H/F",
  "company": "Example Corp",
  "city": "Paris 9e",
  "contract_type": "CDI",
  "salary_min_eur": 55000,
  "salary_max_eur": 70000,
  "remote_policy": "remote_friendly",
  "experience_required": "3-5 ans",
  "description": "Full job description text...",
  "apply_url": "https://www.hellowork.com/fr-fr/emplois/example.html",
  "posted_date": "2026-04-29"
}
```

## Pricing

**PAY_PER_EVENT — $0.001 per job** (= $1 per 1,000). Failed runs not charged.

## FAQ — HelloWork API alternatives

**HelloWork official API?** HelloWork doesn't expose a public job-search API. This scraper is the way to get bulk data.

**Does it cover both `hellowork.com` and the legacy `regionsjob.com`?** Yes — they merged into HelloWork; this scraper queries the unified search.

**Salary fields are often missing — why?** ~40% of French job postings don't show salary publicly. We populate `salary_min_eur` / `salary_max_eur` only when the offer states them explicitly.

**Compatible with HelloWork's `cabinet de recrutement` listings?** Yes — both direct-employer and recruitment-agency offers are returned, distinguished by the `is_agency` flag.

---

⭐ **Found this useful? Bookmark this HelloWork Scraper** — it's the strongest signal for Apify Store ranking.

### Related actors

- [France Travail Scraper](https://apify.com/dltik/francetravail-scraper) — France's largest public job board (700K+ offers)
- [Welcome to the Jungle Scraper](https://apify.com/dltik/welcome-to-the-jungle-scraper) — tech-focused FR/EU jobs
- [JobTeaser Scraper](https://apify.com/dltik/jobteaser-scraper) — student jobs, internships, alternance

License: MIT · Author: [dltik](https://apify.com/dltik)