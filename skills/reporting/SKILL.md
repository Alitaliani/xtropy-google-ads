---
name: google-ads-reporting
description: SEM performance audit report methodology. Use when generating client-facing performance reports, writing report insights, selecting report metrics, or explaining report structure and glossary terms.
---

# SEM Performance Audit Report Methodology

Guidelines for generating automated SEM performance audit reports, including sheet structure, metric definitions, insight writing, and quality assurance.

---

## Report Structure

A standard SEM performance audit report contains these sheets (in order):

| Sheet | Purpose | Data Source Tool |
|---|---|---|
| Monthly Breakout | Per-conversion-action monthly totals | `get_monthly_breakout` |
| SEM Snapshot | Tactic-level summary (Search, PMax, Video) | `get_campaign_performance` (aggregated) |
| Account Overview YTD | Account-level summary + conversion breakout | `get_campaign_performance` + `get_conversion_action_breakout` |
| Campaign Detail YTD | Campaign-level with all conversion actions | `get_campaign_performance` + `get_conversion_action_breakout` |
| Ad Group Overview YTD | Ad group performance | `get_adgroup_performance` |
| Keyword Overview YTD | Keyword-level with match type | `get_quality_scores` |
| Ad Overview YTD | RSA creative with headlines/descriptions | `get_ad_performance` |
| Geography Overview YTD | DMA-level performance | `get_geographic_performance` |
| Glossary | Term definitions | Static + dynamic from data |

Every data sheet includes:
1. **Account Details header** — Account Name, Report Name, Flight Start/End, Report Period, Publishers
2. **Insights paragraph** — AI-generated narrative summarizing key findings
3. **Data table** with consistent metric columns and a Totals row

---

## Metric Definitions

### Standard Metrics (from Google Ads API)

| Metric | API Field | Description |
|---|---|---|
| Impressions | `metrics.impressions` | Number of times an ad was displayed |
| Clicks | `metrics.clicks` | Number of ad clicks |
| Cost | `metrics.cost_micros / 1,000,000` | Total spend in account currency |
| Conversions | `metrics.conversions` | Total conversion actions |
| VTC | `metrics.view_through_conversions` | View-through conversions |

### Derived Metrics (calculated in report)

| Metric | Formula | Format |
|---|---|---|
| CTR | Clicks / Impressions | Percentage (0.0%) |
| CPC | Cost / Clicks | Currency ($0.00) |
| Conv. Rate | Conversions / Clicks | Percentage (0.0%) |
| Cost/Conv. | Cost / Conversions | Currency ($0.00) |
| CPM | Cost / (Impressions / 1,000) | Currency ($0.00) |
| LP Rate | Landing Page Views / Clicks | Percentage (0.0%) |
| CPLPV | Cost / Landing Page Views | Currency ($0.00) |
| CTA Click Rate | CTA Clicks / Clicks | Percentage (0.0%) |

### Tactic Mapping

| Google Ads Channel Type | Report Tactic Label |
|---|---|
| SEARCH | Search |
| PERFORMANCE_MAX | Performance Max |
| VIDEO | Demand Gen Video |
| DISPLAY | Display |
| SHOPPING | Shopping |
| MULTI_CHANNEL | Demand Gen |
| DISCOVERY | Demand Gen |

---

## Insight Writing Guidelines

### Tone and Style

- **Descriptive, not prescriptive** — state what happened, not what to do
- **Specific numbers** — use formatted numbers (e.g., "5.3M impressions", "$51.2K spend")
- **Reference the period** — always mention the date range explicitly
- **Concise** — 2-4 sentences per sheet

### What to Cover Per Sheet

| Sheet | Insight Focus |
|---|---|
| Snapshot | Total volume across tactics, budget allocation, top tactic |
| Account Overview | Account totals, overall CTR, conversion rate, dominant conversion type |
| Campaign Detail | Top 2-3 campaigns by clicks/conversions, standout metrics |
| Ad Group Overview | Best-performing ad groups, conversion rates |
| Keyword Overview | Top keywords by clicks, best conversion rates, notable CTRs |
| Ad Overview | Best-performing RSAs, CTR leaders |
| Geography Overview | Top 2-3 DMAs by volume, concentration percentage, notable CPA differences |
| Monthly Breakout | Month-over-month trends, growing/declining actions |

### Insight Template

```
The {account_name} account from {date_range} earned {impressions} impressions,
{clicks} clicks, and {conversions} conversions. The account had an average CTR
of {ctr} and a conversion rate of {conv_rate}. The majority of conversions were
{top_action} at {count} or {pct}% of all conversions.
```

---

## Report Generation Workflow

### Step 1: Fetch Data (parallel where possible)

```
1. list_accounts → verify customer ID
2. get_account_currency → understand cost units
3. get_campaign_performance(limit=500) → campaigns
4. get_adgroup_performance(limit=500) → ad groups
5. get_quality_scores(limit=500) → keywords
6. get_ad_performance(limit=200) → ad creatives
7. get_geographic_performance(limit=500) → geography
8. get_conversion_action_breakout → per-action metrics
9. get_monthly_breakout → monthly pivot (if needed)
```

### Step 2: Generate Insights

For each sheet, pass the data to the LLM and request a narrative paragraph following the insight writing guidelines above.

### Step 3: Generate Report

Call `generate_sem_report` with all fetched data and insights. The tool produces the styled Excel workbook.

### Step 4: Quality Check

- [ ] Open the generated file and verify sheet count
- [ ] Check that totals rows match expected sums
- [ ] Verify no real customer IDs appear in demo/test outputs
- [ ] Confirm number formatting (percentages, currency, thousands separators)

---

## Number Formatting Rules

| Data Type | Format | Example |
|---|---|---|
| Impressions, Clicks | `#,##0` | 5,373,660 |
| Cost, CPC, Cost/Conv | `$#,##0.00` | $51,218.68 |
| CTR, Conv Rate, LP Rate | `0.0%` | 3.3% |
| Conversions | `#,##0.00` | 135,941.42 |
| CPM, CPLPV | `$#,##0.00` | $24.00 |

---

> **Built by [Xtropy](https://www.xtropy.net).** SEM performance reporting methodology for agency management.
