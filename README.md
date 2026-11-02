# Zepto Scraper

Extract structured data from [zepto.com](https://zepto.com) — Zepto grocery search results in India by keyword and delivery point. Each product row carries name, brand and pack size with MRP and selling price in INR plus discount and stock status. Rating, images and the serving store's delivery ETA are included too.

**[Zepto Scraper on Apify →](https://apify.com/blackfalcondata/zepto-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from zepto.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from zepto.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "milk",
  "maxResults": 100,
  "onlyOffers": false,
  "inStockOnly": false,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Product keywords to search for. Use a JSON array for several, e.g. ["milk","atta"]. |
| `location` | string | — | Prices, stock and delivery time are per store. Give a delivery point as an area or landmark (e.g. "Bandra West, Mumbai"), a 6-digit PIN code (e.g. "400050") or "lat,lng" (e.g. "19.0760,72.8777"). A place Zepto cannot find or does not deliver to stops the run with a clear message. Use a JSON array for several. Leave empty for the default store. |
| `maxResults` | integer | `100` | Maximum products in total (0 = everything). |
| `startUrls` | array | `[]` | Optional: paste zepto.com search or category URLs. Each URL becomes its own task; results are de-duplicated by product. |
| `onlyOffers` | boolean | `false` | Drop products at their regular price. |
| `priceMin` | number | — | Lowest shelf price to keep. |
| `priceMax` | number | — | Highest shelf price to keep. |
| `inStockOnly` | boolean | `false` | Drop products the shop marks out of stock. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a hash of the search inputs (keywords, delivery points and filters) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit records that haven't changed. |
| `emitExpired` | boolean | `false` | When incremental, also emit records no longer found. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches an expired product from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
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

---

## Output fields

Every product returns the same 59-field schema. Missing values are `null` — never omitted.

- `product_variant_id`
- `product_id`
- `store_product_id`
- `product_url`
- `portalUrl`
- `name`
- `brand`
- `brand_id`
- `description`
- `ingredients`
- `product_type`
- `is_active`
- `formatted_packsize`
- `packsize`
- `unit_of_measure`
- `weight_in_gms`
- `mrp`
- `selling_price`
- `discounted_selling_price`
- `super_saver_selling_price`
- `zepto_pass_price`
- `discount_amount`
- `discount_percent`
- `currency`
- `out_of_stock`
- `available_quantity`
- `max_allowed_quantity`
- `catalog_quantity`
- `is_best_offer`
- `is_new_product`
- `rating_average`
- `rating_count`
- `primary_category_name`
- `primary_category_id`
- `primary_subcategory_name`
- `primary_subcategory_id`
- `manufacturer_name`
- `manufacturer_address`
- `country_of_origin`
- `fssai_license`
- `shelf_life_hours`
- `attribute_tags`
- `image_url`
- `images`
- `store_id`
- `eta_in_minutes`
- `location_label`
- `latitude`
- `longitude`
- `search_query`
- `page_number`
- `scraped_at`
- `searchQuery`
- `scrapedAt`
- `contentHash`
- `detailFetched`
- `source`
- `offerChange`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "product_variant_id": "25eb526c-9c26-48cd-95a3-8e4058910f8a",
  "product_id": "5dac97a5-0241-4b24-9a5d-8243838257f5",
  "store_product_id": "816126c5-3fa9-5706-be57-c2b3ff7ceced",
  "product_url": "https://www.zepto.com/pn/nandini-toned-fresh-milk-pouch/pvid/25eb526c-9c26-48cd-95a3-8e4058910f8a",
  "portalUrl": "https://www.zepto.com/pn/nandini-toned-fresh-milk-pouch/pvid/25eb526c-9c26-48cd-95a3-8e4058910f8a",
  "name": "Nandini Toned Fresh Milk | Pouch",
  "brand": "Nandini",
  "brand_id": "ef77c6aa-2f07-4843-a955-7972f6d069d7",
  "description": "Karnatakas highest selling and most preferred milk. Pasteurised Toned milk with Min. 3.0% fat and Min.8.5 % SNF content make this milk the best choice for all purposes and all gene…",
  "ingredients": null,
  "product_type": "SELLABLESKU",
  "is_active": true
}
```

*Truncated — full records contain 59 fields. See Output fields for the complete schema.*

**[Try Zepto Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/zepto-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00045 |
| Result | $0.00072 |

See the [actor on Apify](https://apify.com/blackfalcondata/zepto-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape zepto.com?**
Use this actor on Apify to extract structured data from zepto.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get zepto.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape zepto.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check zepto.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Zepto Scraper](https://apify.com/blackfalcondata/zepto-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
