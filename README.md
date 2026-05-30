# Google Maps Lead Scraper — Emails, Phones & B2B Leads

Turn any Google Maps search into a contact-ready B2B lead list: business name,
address, phone, **website email**, and an **agency-fit lead score (0–100)** that
ranks which prospects are most likely to need help (weak online presence first).

For agencies, SDRs, local SEO consultants, and cold-email / sales prospecting.

## Why email + score matter

Most Google Maps scrapers stop at name and phone. A phone number alone is a cold
dial. To run real cold email you need a **contact email**, and to spend time well
you want to **sort by opportunity** — businesses with no website, few reviews, or
an unclaimed listing are the easiest "we can help you get found" conversations.

This tool visits each business website to pull a contact email, then computes the
agency-fit score so you call the best leads first.

## Two ways to use it

### 1. DIY — run it yourself (pay per result)

The hosted actor on Apify drives a real browser (Google Maps is JS-rendered, so
plain HTTP scrapers return empty pages), enriches each result with an email + score,
and exports JSON/CSV. No API key, no infrastructure.

➡️ **[Run on Apify](https://apify.com/lazymac/google-maps-business-scraper)**

```bash
# via Apify API (APIFY_TOKEN from apify.com/account/integrations)
curl -X POST "https://api.apify.com/v2/acts/lazymac~google-maps-business-scraper/runs?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"searchQuery": "family dentists in Phoenix AZ", "maxResults": 200}'
```

Output columns per business: `title, address, phone, website, email, agencyFitScore,
totalScore, reviewsCount, categoryName, latitude, longitude, placeId` + more.

### 2. Done-for-you — just get the CSV

Don't want to run anything? Order a niche + city and get a ready-to-import CSV of
**500 verified leads within 24 hours**.

➡️ **[500 Verified Local Business Leads — $39](https://coindany.gumroad.com/l/local-leads-500)**

## How to use the agency-fit score

1. Sort descending by `agencyFitScore`.
2. Top rows = weakest web presence = easiest prospects.
3. `email` column is your cold-email list; for calling, work top-of-score first.

Search → enrich with email + score → sort → outreach. The data stops being the bottleneck.

---

*Questions on a specific niche or city? Open an issue.*
