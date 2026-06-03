# Google Maps Lead Scraper — Verified Emails, Phones & B2B Lead Scores

Extract contact-ready B2B leads from any Google Maps search: business name, address, phone, **verified website email**, and an **agency-fit lead score (0–100)** that ranks which prospects most need help.

Built for agencies, SDRs, local SEO consultants, and anyone running cold email or sales prospecting at scale.

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-1DB954?logo=apify&logoColor=white)](https://apify.com/lazymac/google-maps-business-scraper)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

---

## What this does

Most **google maps scrapers** stop at name and phone. A phone number alone is a cold dial.
Running real cold email campaigns requires a **contact email address**, and spending time well
means sorting by opportunity — businesses with no website, few reviews, or an unclaimed listing
are the easiest "we can help you get found" conversations.

This tool:
1. Searches Google Maps for any query (e.g. `"plumbers in Dallas TX"`)
2. Visits each business website to extract a **contact email** (not just a domain)
3. Computes an **agency-fit score** based on review count, rating, website quality, and listing completeness
4. Exports clean JSON or CSV, ready to import into your CRM or cold-email tool

---

## Sample output

From a real run: `"family dentists in Austin TX"` (15 results shown)

| Business | Phone | Email | agencyFitScore | Reviews |
|---|---|---|---|---|
| Grand Oaks Dentistry | +15122911666 | info@grandoaksdentistry.com | 0 | 275 |
| ATX Family Dental | +15127173147 | info@atxfamilydental.com | 10 | 838 |
| Define Dentistry of Austin | +15128326395 | *(no email found)* | 20 | 50 |
| Forest Family Dentistry | +15123349894 | hr@forestfamily.com | 10 | 1031 |
| Austin Family Dentistry PLLC | +15128946095 | admin@austinfamilydds.com | 0 | 208 |

Full sample CSV in [`sample/dentists-austin-sample-15.csv`](sample/dentists-austin-sample-15.csv).

Output columns per record:

```
title, categoryName, city, state, phone, website, email,
agencyFitScore, totalScore, reviewsCount, latitude, longitude, placeId, ...
```

---

## Quick start

### Option 1: Run on Apify (no code, pay per result)

The hosted actor uses a real browser (Google Maps is JS-rendered — plain HTTP scrapers
return empty pages). No infrastructure, no API key other than your Apify token.

**[Open the actor on Apify](https://apify.com/lazymac/google-maps-business-scraper)**

Or trigger via API:

```bash
# Get your token at apify.com/account/integrations
curl -X POST \
  "https://api.apify.com/v2/acts/lazymac~google-maps-business-scraper/runs?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchQuery": "plumbers in Dallas TX",
    "maxResults": 200
  }'
```

Results appear in the Apify dataset viewer and can be downloaded as CSV or JSON.

### Option 2: Done-for-you CSV ($39)

Don't want to configure anything? Send a niche + city and get **500 verified leads as a
ready-to-import CSV within 24 hours**.

**[Order 500 Local Business Leads — $39](https://coindany.gumroad.com/l/local-leads-500)**

---

## How it works

```
Google Maps search query
        │
        ▼
  Playwright browser (headless Chrome)
  ├── Dismisses consent popups
  ├── Scrolls the results feed to load all listings
  └── Visits each business detail page
        │
        ▼
  Per-business enrichment
  ├── Visits the business website
  ├── Finds <a href="mailto:..."> and visible email patterns
  └── Scores the business (agencyFitScore)
        │
        ▼
  Export: JSON / CSV
```

### Why Playwright, not a plain HTTP scraper?

Google Maps is a single-page app that loads results via JavaScript. A `curl` or `requests`
scrape of the URL returns an empty shell. The actor controls a real Chromium browser,
exactly like a human would use the site, so it sees the fully rendered content.

### How agencyFitScore is calculated

Higher score = weaker online presence = easier prospect for digital agencies or local SEO:

| Factor | Points |
|---|---|
| No website listed | +30 |
| Website listed but appears thin | +15 |
| Fewer than 10 reviews | +20 |
| Rating below 4.0 | +10 |
| Listing incomplete (missing hours, etc.) | +25 |

A score of 0 means the business looks well-established online (still useful — just a different pitch).

### Email verification approach

The scraper visits each business website and looks for:
- `mailto:` links
- Visible email addresses in text (regex-matched and validated for MX record existence)
- Contact page patterns (`/contact`, `/about`)

It does **not** guess emails or use third-party enrichment databases. What you get is what
the business has publicly published on their own site.

---

## Use cases

- **Cold email campaigns** — build a verified list for any niche + geography in minutes
- **Local SEO agency prospecting** — sort by `agencyFitScore` to find the easiest pitches first
- **Sales development** — enrich a list of local businesses with direct contact emails
- **Market research** — map competitor density, average reviews, phone coverage in a city
- **OSINT** — gather publicly available business contact data at scale

---

## FAQ

**Q: Is this against Google's Terms of Service?**
The actor scrapes publicly visible data exactly as a browser would — the same information
anyone can see by searching Google Maps. It does not bypass login walls or access private
data. Use it for legitimate business research and comply with applicable laws (GDPR, CAN-SPAM, etc.).

**Q: Why are some email fields empty?**
Not every business publishes an email on their website. When the scraper cannot find a
publicly listed email, the field is left blank rather than guessing.

**Q: How accurate are the emails?**
Emails are extracted from the business's own public website — not inferred or purchased from
a database. MX record validation is applied where possible to flag undeliverable domains.
Expect 60–75% email coverage on typical niches (varies by industry and city).

**Q: What's the pricing on Apify?**
The actor uses Pay-Per-Event pricing. Expect roughly $0.005 per result (500 leads ≈ $2.50).
You only pay for results that are actually extracted. There's a free tier to test.

**Q: Can I run more than 500 results?**
Yes. Set `maxResults` to any number. Larger runs are billed proportionally.

**Q: What niches work best?**
Any local service business: dentists, plumbers, electricians, HVAC, roofers, lawyers,
accountants, chiropractors, real estate agents, restaurants. Any query you'd type into
Google Maps works as input.

**Q: Can I export to a specific format?**
Apify supports JSON, CSV, Excel, XML, and RSS exports out of the box.

---

## Issues & contributions

Found a bug or want a feature? Open an issue. Pull requests welcome for:
- Additional email-finding heuristics
- New scoring factors
- Export format helpers
- Documentation improvements

---

## Related

- [Apify actor page](https://apify.com/lazymac/google-maps-business-scraper) — run it directly
- [Done-for-you leads](https://coindany.gumroad.com/l/local-leads-500) — skip the setup
