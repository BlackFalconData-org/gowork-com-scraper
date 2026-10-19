# GoWork Scraper

Extract structured data from [GoWork companies and their employee reviews](https://gowork.fr) — GoWork companies and their employee reviews: ratings, rating breakdown, contact and registry details across France, Spain, Germany, Italy and the UK. Search by name or city, or paste a company link. Incremental mode returns only new and changed companies.

**[GoWork Scraper on Apify →](https://apify.com/blackfalcondata/gowork-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 🌍 country, 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, employer profiles, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from GoWork companies and their employee reviews on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from GoWork companies and their employee reviews.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "carrefour",
  "country": "fr",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Company name or keyword to search for. Use * or leave empty with a city to list all companies. |
| `location` | string | — | Optional city to narrow the search (e.g. Paris). |
| `country` | enum | `"fr"` | Which GoWork site to search. |
| `maxResults` | integer | `100` | Maximum number of companies to return (0 = unlimited). Reviews are set separately and do not count towards this number. |
| `startUrls` | array | `[]` | Paste GoWork company pages (for example https://gowork.fr/carrefour) or search pages from any of the five sites. A company page returns that company and its reviews; a search page runs that search. Unsupported links are skipped with a warning. When provided, start URLs replace the query and city fields. |
| `includeDetails` | boolean | `true` | Fetch each company profile for the rating breakdown, region and registry details. |
| `includeReviews` | boolean | `true` | Return each company's employee reviews as separate rows (record type "review") that carry the company name and ID. Turn off for company profiles only. |
| `maxReviewsPerCompany` | integer | `20` | How many reviews to return for each company. 0 = every review. |
| `onlyRatedReviews` | boolean | `false` | Skip reviews that carry no star rating. |
| `reviewsStartDate` | string | — | Only return reviews written on or after this date (YYYY-MM-DD). |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `emitUnchanged` | boolean | `false` | When Incremental Mode is on, also return companies that have not changed since the last run. Off by default: unchanged companies are skipped. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip companies whose content matches an expired company from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED companies. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of companies included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `lat` | number | — | Latitude of the centre point. Pair with Longitude. Companies get a distance in kilometres; add a radius to keep only nearby companies. Companies without a published location are kept. |
| `lon` | number | — | Longitude of the centre point. Pair with Latitude. |
| `radiusKm` | integer | — | Keep only companies within this many kilometres of the latitude and longitude. Leave empty to only add the distance. |

---

## Output fields

Every record returns the same 54-field schema. Missing values are `null` — never omitted.

- `recordType`
- `companyId`
- `companyName`
- `tradeName`
- `slug`
- `companySlug`
- `companyUrl`
- `description`
- `website`
- `email`
- `phone`
- `address`
- `city`
- `region`
- `postalCode`
- `businessArea`
- `activityCode`
- `legalForm`
- `employeeCount`
- `taxId`
- `logoUrl`
- `overallRating`
- `reviewCount`
- `ratingCount`
- `ratingDistribution`
- `reviewsType`
- `lastReviewDate`
- `isGold`
- `multiCity`
- `countryCode`
- `language`
- `scrapedAt`
- `listingId`
- `profileUrl`
- `employerName`
- `phoneNumber`
- `logoLink`
- `streetLine`
- `cityName`
- `regionName`
- `sectorName`
- `ratingsTotal`
- `reviewsTotal`
- `starSplit`
- `pageSummary`
- `portalUrl`
- `distanceKm`
- `collectedAt`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "recordType": "company",
  "companyId": "649aee8c-bdf1-4ca2-eb16-83686d1a7927",
  "companyName": "CARREFOUR CONTACT",
  "tradeName": "Commerce",
  "slug": "carrefour-contact-hoerdt",
  "companySlug": "carrefour-contact-hoerdt",
  "companyUrl": "https://gowork.fr/carrefour-contact-hoerdt",
  "description": "Très reactif, service impeccable, rayon jamais vide et rayo...",
  "website": "https://www.carrefour.fr",
  "email": null,
  "phone": "+33 3 88 51 78 20",
  "address": "108 RUE DE LA REPUBLIQUE, 67720 HOERDT"
}
```

*Truncated — full records contain 54 fields. See Output fields for the complete schema.*

**[Try GoWork Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/gowork-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0081 |
| Result | $0.0009 |

See the [actor on Apify](https://apify.com/blackfalcondata/gowork-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape GoWork companies and their employee reviews?**
Use this actor on Apify to extract structured data from gowork.fr. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get GoWork companies and their employee reviews data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape GoWork companies and their employee reviews?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check GoWork companies and their employee reviews's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [GoWork Scraper](https://apify.com/blackfalcondata/gowork-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
