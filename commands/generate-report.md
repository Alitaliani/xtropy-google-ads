# Generate Filtered SEM Report

Generate a full SEM performance audit report with an explicit date range and conversion action filtering.

## Report Parameters

$ARGUMENTS — account name or ID, date range, and conversion action filter patterns.

Examples:
- "Client A 1/19/26-3/17/26 only CARE, ESA, LIHEAP conversions"
- "1112223333 last 30 days filter to Engaged Visit and CARE"
- "Client B YTD all conversions"

## Workflow

### Step 1: Parse Arguments

Extract from $ARGUMENTS:
- **Account**: name or customer ID
- **Date range**: start and end dates. Parse flexible formats: "1/19/26-3/17/26", "last 30 days", "YTD", "Jan 19 - Mar 17 2026". Convert to YYYY-MM-DD format for GAQL queries.
- **Conversion filter**: list of substring patterns (e.g., ["CARE", "ESA", "LIHEAP"]). If user says "all conversions" or doesn't specify a filter, use an empty list to show all.

If any required parameter is ambiguous, ask the user to clarify before proceeding.

### Step 2: Resolve Account

1. Call `list_accounts` to get accessible accounts
2. Resolve account name to customer_id (same pattern as list-campaigns: match by ID or substring on descriptive_name)
3. Call `get_account_currency`

### Step 3: Fetch All Data

Use `run_gaql` with explicit `BETWEEN '{start_date}' AND '{end_date}'` for all queries to ensure the exact date range is honored. Run queries in parallel where possible.

**Parallel batch 1** (metric queries with date range):

a. **Campaign data**:
   ```sql
   SELECT campaign.id, campaign.name, campaign.status,
     campaign.advertising_channel_type, campaign.advertising_channel_sub_type,
     metrics.impressions, metrics.clicks, metrics.cost_micros,
     metrics.conversions, metrics.conversions_value, metrics.average_cpc,
     metrics.ctr, metrics.view_through_conversions
   FROM campaign
   WHERE segments.date BETWEEN '{start}' AND '{end}'
   ORDER BY metrics.cost_micros DESC LIMIT 500
   ```

b. **Ad group data**:
   ```sql
   SELECT campaign.name, ad_group.id, ad_group.name, ad_group.status,
     metrics.impressions, metrics.clicks, metrics.cost_micros,
     metrics.conversions, metrics.conversions_value, metrics.ctr,
     metrics.average_cpc, metrics.cost_per_conversion,
     metrics.view_through_conversions
   FROM ad_group
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND ad_group.status != 'REMOVED' AND metrics.impressions > 0
   ORDER BY metrics.clicks DESC LIMIT 500
   ```

c. **Keyword data**:
   ```sql
   SELECT ad_group_criterion.keyword.text,
     ad_group_criterion.keyword.match_type,
     ad_group_criterion.quality_info.quality_score,
     campaign.name, ad_group.name,
     metrics.impressions, metrics.clicks, metrics.cost_micros,
     metrics.conversions, metrics.ctr, metrics.average_cpc,
     metrics.view_through_conversions
   FROM keyword_view
   WHERE ad_group_criterion.status = 'ENABLED'
     AND campaign.status = 'ENABLED'
   ORDER BY metrics.cost_micros DESC LIMIT 500
   ```

d. **Ad creative data**:
   ```sql
   SELECT ad_group_ad.ad.id, ad_group_ad.ad.name, ad_group_ad.ad.type,
     ad_group_ad.status, ad_group_ad.ad.final_urls,
     ad_group_ad.ad.responsive_search_ad.headlines,
     ad_group_ad.ad.responsive_search_ad.descriptions,
     ad_group_ad.ad.responsive_search_ad.path1,
     ad_group_ad.ad.responsive_search_ad.path2,
     ad_group_ad.ad_strength, campaign.name, ad_group.name,
     metrics.impressions, metrics.clicks, metrics.cost_micros,
     metrics.conversions, metrics.ctr, metrics.average_cpc,
     metrics.cost_per_conversion, metrics.view_through_conversions
   FROM ad_group_ad
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND metrics.impressions > 0
     AND ad_group_ad.ad.type = 'RESPONSIVE_SEARCH_AD'
   ORDER BY metrics.impressions DESC LIMIT 200
   ```

e. **Geographic data** (two-step — location_view cannot join geo_target_constant):
   ```sql
   SELECT campaign_criterion.location.geo_target_constant,
     metrics.impressions, metrics.clicks, metrics.cost_micros,
     metrics.conversions, metrics.conversions_value, metrics.ctr,
     metrics.average_cpc, metrics.cost_per_conversion,
     metrics.view_through_conversions
   FROM location_view
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND metrics.impressions > 0
   ORDER BY metrics.clicks DESC LIMIT 200
   ```
   Then resolve each geo ID separately via `run_gaql`:
   ```sql
   SELECT geo_target_constant.name, geo_target_constant.canonical_name,
     geo_target_constant.target_type
   FROM geo_target_constant
   WHERE geo_target_constant.resource_name = 'geoTargetConstants/{id}'
   ```

f. **Conversion action breakout**:
   ```sql
   SELECT campaign.name, segments.conversion_action_name,
     metrics.conversions, metrics.conversions_value,
     metrics.view_through_conversions
   FROM campaign
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND metrics.conversions > 0
   ORDER BY campaign.name
   ```

g. **Monthly breakout**: Call `get_monthly_breakout` with start_date and end_date in YYYY-MM-DD format (this tool already accepts explicit dates).

h. **Device data**:
   ```sql
   SELECT segments.device, metrics.impressions, metrics.clicks,
     metrics.cost_micros, metrics.conversions, metrics.conversions_value,
     metrics.ctr, metrics.average_cpc, metrics.view_through_conversions
   FROM campaign
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND metrics.impressions > 0
   ```

i. **Extension data**:
   ```sql
   SELECT asset.type, metrics.impressions, metrics.clicks,
     metrics.cost_micros, metrics.conversions, metrics.ctr,
     metrics.average_cpc
   FROM ad_group_asset
   WHERE segments.date BETWEEN '{start}' AND '{end}'
     AND metrics.impressions > 0
   ORDER BY metrics.impressions DESC LIMIT 200
   ```

**Parallel batch 2** (asset queries — no date filter):

j. **Image assets**:
   ```sql
   SELECT asset.id, asset.name, asset.type,
     asset.image_asset.full_size.url,
     asset.image_asset.full_size.height_pixels,
     asset.image_asset.full_size.width_pixels,
     asset.image_asset.file_size
   FROM asset WHERE asset.type = 'IMAGE' LIMIT 20
   ```

k. **PMax assets**:
   ```sql
   SELECT asset_group.name, asset_group_asset.field_type,
     asset.id, asset.name, asset.type,
     asset.image_asset.full_size.url,
     asset.image_asset.full_size.width_pixels,
     asset.image_asset.full_size.height_pixels,
     campaign.name
   FROM asset_group_asset
   WHERE asset_group_asset.field_type IN ('MARKETING_IMAGE',
     'SQUARE_MARKETING_IMAGE', 'PORTRAIT_MARKETING_IMAGE',
     'LOGO', 'LANDSCAPE_LOGO')
   LIMIT 50
   ```

l. **Video assets**:
   ```sql
   SELECT asset.id, asset.type,
     asset.youtube_video_asset.youtube_video_id,
     asset.youtube_video_asset.youtube_video_title
   FROM asset WHERE asset.type = 'YOUTUBE_VIDEO' LIMIT 10
   ```

### Step 4: Generate Insights

For each sheet, write 2-4 sentence insights summarizing the data:
- Focus on key numbers, trends, and standout performers
- Be descriptive, not prescriptive (no recommendations)
- Reference specific campaigns/keywords by name
- Use formatted numbers (e.g., "5.3M impressions", "$51.2K spend")

Sheet keys for insights: `snapshot`, `account_overview`, `campaign_detail`, `adgroup_overview`, `keyword_overview`, `ad_overview`, `geography_overview`, `monthly_breakout`

### Step 5: Generate Report

Call `generate_sem_report` with:
- All fetched data as JSON strings
- `sheets`: ALL 12 sheets:
  ```json
  ["snapshot", "account_overview", "campaign_detail", "adgroup_overview",
   "keyword_overview", "ad_overview", "geography_overview",
   "device_overview", "extension_performance",
   "monthly_breakout", "creatives", "glossary"]
  ```
- `conversion_action_filter`: the parsed filter patterns as a JSON array (e.g., `["CARE", "ESA", "LIHEAP"]`). Use `[]` for no filter.
- `report_period`: formatted as "M/D/YY-M/D/YY" from the parsed dates
- `output_dir`: "reports"

### Step 6: Upload & Notify

Call `upload_report` with:
- `file_path`: from generate_sem_report response
- `account_name`: the account display name
- `report_period`: the date range label
- `sheet_count`: number of sheets generated

This uploads to Google Drive (restricted access), and sends Slack + Gmail notifications.

### Step 7: Report Result

Display:
- Google Drive link (restricted to authorized recipients)
- Number of sheets generated (should be 12 + Report Settings sheet)
- Which conversion actions were included vs. excluded (from the Settings sheet)
- Notification status (Slack sent, Gmail sent)
