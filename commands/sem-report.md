# SEM Performance Audit Report

Generate a comprehensive SEM performance audit report as a styled Excel workbook.

## Target Account

$ARGUMENTS — provide a customer ID (e.g., 123-456-7890) or account name, plus optional date range (e.g., "1/19/26-3/1/26") and sheets to include.

## Report Generation Workflow

1. **Discover account**: Use `list_accounts` to verify the target account exists and get its customer ID
2. **Get currency**: Use `get_account_currency` to understand cost units before analyzing spend data
3. **Fetch data** (run these in parallel where possible):
   a. `get_campaign_performance` with `limit=500` for full account data
   b. `get_adgroup_performance` with `limit=500`
   c. `get_quality_scores` with `limit=500` for keyword-level data
   d. `get_ad_performance` with `limit=200` for RSA creative details
   e. `get_geographic_performance` with `limit=500` for DMA-level data
   f. `get_conversion_action_breakout` for per-campaign conversion action breakout
   g. `get_monthly_breakout` with start/end dates (if Monthly Breakout sheet requested)
   h. `get_ga4_landing_page_stats` (if GA4 connected, for LPV data)
4. **Generate insights**: For each sheet's data, write a narrative paragraph:
   - Summarize key metrics with formatted numbers (e.g., "5.3M impressions", "$51.2K spend")
   - Call out top 2-3 performers by the primary metric
   - Note significant trends or anomalies
   - Match tone: descriptive (what happened), not prescriptive (what to do)
   - Keep to 2-4 sentences per sheet
5. **Generate report**: Call `generate_sem_report` with all fetched data + insights JSON dict
6. **Upload & notify**: Call `upload_report` with the file_path, account_name, report_period, and sheet_count to upload to Google Drive (restricted access) and send Slack + Gmail notifications
7. **Deliver**: Return the Drive link and sheet count

## Sheets Available

- `snapshot` — Tactic-level summary (Search, PMax, Video)
- `account_overview` — Account-level summary with conversion action breakout
- `campaign_detail` — Campaign-level with all conversion action columns
- `adgroup_overview` — Ad group performance
- `keyword_overview` — Keyword-level with match type and quality data
- `ad_overview` — RSA creative with headlines/descriptions
- `geography_overview` — DMA-level geographic performance
- `monthly_breakout` — Per-conversion-action monthly totals pivot
- `glossary` — Standard + account-specific term definitions

## Insight Writing Guidelines

For each sheet, write 2-4 sentences covering:
- Total volume (impressions, clicks, conversions) with the timeframe
- Key rates (CTR, conversion rate) with context
- Top 2-3 performers by the primary metric
- Any notable findings (high cost/low conversion, geographic concentration)

Use specific numbers formatted for readability. Reference the report period explicitly.

## Example Usage

```
/google-ads:sem-report 123-456-7890 1/19/26-3/1/26
/google-ads:sem-report "CARE" YTD
/google-ads:sem-report 123-456-7890 last 30 days --sheets snapshot,campaign_detail
```
