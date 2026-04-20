# CLAUDE.md — Xtropy Google Ads MCP Navigation Guide

> **Where this file goes:** save as `~/.claude/CLAUDE.md` on your machine (applies to every project you work in). Or drop it inside a specific project as `.claude/CLAUDE.md` if you only want it active there.
>
> **Why it exists:** Claude Code loads this file into context at session start. Without it, Claude discovers tools and skills by making round-trip calls to the MCP server — which is slow. With it, Claude knows the toolkit upfront and only reaches for server-side methodology when a strategic question actually comes up.

---

## You are connected to the Xtropy Google Ads MCP

Hosted at `https://mcp.trafficarchitect.com/mcp`. You have 54 Google Ads tools and 8 methodology skills. Use them directly — no special preamble needed.

When uncertain which tool fits, pick from the groups below. For strategic reasoning (*"is this the right campaign structure?"*), load the matching `skill_*` prompt on demand.

---

## Tool Map — by intent

### Discovery (start here)

| Intent | Tool |
|---|---|
| Which accounts can I see? | `list_accounts` |
| Full MCC client tree with currencies | `discover_accounts` |
| What's an account's currency? | `get_account_currency` |
| Recent campaign/budget/bid changes | `get_change_history` |

### Performance — last N days

| Intent | Tool |
|---|---|
| Campaign-level numbers | `get_campaign_performance` |
| Ad group drilldown | `get_adgroup_performance` |
| Ad creatives + metrics | `get_ad_performance` (pass `compact=true` for large accounts) |
| Keyword-level performance | `get_search_terms` |
| Quality Scores | `get_quality_scores` (reduce `limit` for very large accounts) |
| Budget pacing | `get_budget_pacing` |
| Impression-share / competitive position | `get_competitive_position` |
| Pending Google recommendations | `get_recommendations` |

### Segments

| Intent | Tool |
|---|---|
| Device split | `get_device_performance` |
| Hour-of-day / day-of-week | `get_hourly_performance` |
| Extensions (sitelinks/callouts/calls) | `get_extension_performance` |
| Audience performance | `get_audience_performance` |
| Bidding strategy performance | `get_bidding_strategies` |

### Report building (for `generate_sem_report`)

| Intent | Tool |
|---|---|
| Geographic split | `get_geographic_performance` |
| Monthly conversion breakout | `get_monthly_breakout` — pass `months=3` or explicit dates |
| Conversion-action breakout | `get_conversion_action_breakout` |

### Assets & creatives

| Intent | Tool |
|---|---|
| Standalone image assets | `get_image_assets` |
| Asset usage across accounts | `get_asset_usage` |
| AI-analyze image creatives | `analyze_image_assets` |

### Write operations (always safe — default `dry_run=True`)

| Intent | Tool |
|---|---|
| List current negative keywords | `list_negative_keywords` |
| Add negatives | `add_negative_keywords` |
| Remove negatives | `remove_negative_keywords` |
| Apply a Google recommendation | `apply_recommendation` |

### Planning

| Intent | Tool |
|---|---|
| Keyword ideas, volumes, competition | `get_keyword_ideas` |

### Reports — end-to-end

| Intent | Tool |
|---|---|
| Full SEM Excel audit (1 account) | `generate_sem_report` |
| Weekly audits across all accounts | `run_weekly_reports` |
| Upload + Drive + notify | `upload_report` (auto-falls-back to Drive API when rclone isn't available) |
| Save a named report config | `save_report_config` · `list_report_configs` · `get_report_config` · `delete_report_config` |

### External integrations

| Intent | Tool |
|---|---|
| GA4 conversions + landing pages | `get_ga4_performance` · `get_ga4_landing_page_stats` |
| Audit GA4↔Ads conversion tracking | `audit_conversion_tracking` |
| Landing page speed + on-page audit | `analyze_landing_pages` · `check_page_speed` |
| Competitor SERPs for a keyword | `audit_serp` · `audit_serp_multi` · `serp_search` · `serp_search_batch` |

### Feedback (after meaningful outputs)

| Intent | Tool |
|---|---|
| Record a 1–5 rating | `submit_feedback` |

### Raw escape hatch

| Intent | Tool |
|---|---|
| Run any GAQL query | `run_gaql` |

---

## Methodology Skills — load only when the question is strategic

These are detailed playbooks hosted server-side. Don't load them for routine data pulls. Fetch them via `prompts/get` when the user asks *how* or *why*, not just *what*.

| Skill | Load when |
|---|---|
| `skill_strategy` | Campaign type choice (PMax vs Search vs Shopping), account structure, bidding philosophy, QS thinking |
| `skill_campaigns` | Naming, ad-group structure, RSA/Shopping/PMax setup, extensions |
| `skill_optimization` | Weekly optimization, Account Hygiene Checklist, scale-vs-cut decisions |
| `skill_audiences` | RLSA, Customer Match, in-market, remarketing |
| `skill_tracking` | Conversion tracking, GTM, enhanced conversions, offline import |
| `skill_reporting` | SEM report methodology, what goes in each sheet |
| `skill_feedback` | How to interpret `_feedback` hints on tool responses |

---

## The three most common workflows

### Prospect / client audit — 5 minutes

1. `list_accounts` → confirm the account id
2. `get_campaign_performance(customer_id, days=30)` → overview
3. `get_search_terms(customer_id, days=30, min_cost_micros=20000000)` → wasted spend
4. `get_quality_scores(customer_id, limit=50)` → QS red flags
5. `get_competitive_position(customer_id, days=30)` → impression-share lost
6. `get_recommendations(customer_id)` → Google's pending suggestions
7. Synthesize into a 5-finding summary. If asked to write a full Excel, call `generate_sem_report` with the data you just pulled.

### Weekly audit across all active accounts

- One call: `run_weekly_reports(report_mode="audit", skip_inactive=true, summary_email="team@xtropy.net")`
- It loops through all live MCC accounts, generates 4-sheet audits per account, uploads to Drive, emails a consolidated summary, and emits per-account `report_generated` events you'll see on the admin dashboard.

### Full SEM performance report for one account

1. Fetch data in parallel: `get_campaign_performance`, `get_adgroup_performance`, `get_ad_performance(compact=true)`, `get_geographic_performance`, `get_device_performance`, `get_conversion_action_breakout`, `get_monthly_breakout(months=3)`
2. Pass the JSON outputs to `generate_sem_report` with the sheets list
3. `upload_report` to push to Drive + notify
4. Share the `download_url` from either response with the user — they can pull the .xlsx to their laptop directly

Check the response for `sheets_skipped` — if a requested sheet dropped, the response tells you which parameter was empty.

### Downloading the generated .xlsx

Every `generate_sem_report` and `upload_report` response includes a `download_url` like `https://mcp.trafficarchitect.com/download/PG_E_Safety_2026-04-17_18-08.xlsx`.

The user fetches it with their own API key:

```bash
curl -OJ -H "Authorization: Bearer YOUR_KEY" \
  https://mcp.trafficarchitect.com/download/PG_E_Safety_2026-04-17_18-08.xlsx
```

PowerShell:
```powershell
Invoke-WebRequest -Uri "https://mcp.trafficarchitect.com/download/FILE.xlsx" `
  -Headers @{ Authorization = "Bearer YOUR_KEY" } `
  -OutFile "FILE.xlsx"
```

Two important caveats:
1. The file lives on the Cloud Run container's local disk. **Download within ~15 minutes** of generation, before the container recycles. If it fails with 404, just regenerate.
2. When Drive upload succeeds, prefer `drive_link` for sharing with external stakeholders — the direct download requires the bearer token.

---

## Response handling conventions

### `_auto_download` hints

When a tool response contains an `_auto_download` object, **fetch it immediately** and save to the user's current working directory. Do NOT wait for the user to ask — auto-download is the expected behavior whenever the hint is present.

```json
{
  "_auto_download": {
    "url": "https://mcp.trafficarchitect.com/download/PG_E_Safety_2026-04-17.xlsx?token=...",
    "filename": "PG_E_Safety_2026-04-17.xlsx",
    "expires_in_seconds": 900,
    "action": "Fetch this URL and save it to the user's current working directory..."
  }
}
```

1. Parse `url` and `filename`
2. Run via the `Bash` tool: `curl -fsSL -o "$filename" "$url"` (URL embeds a short-lived token — no auth header needed)
3. Confirm to the user: *"Saved to ./FILE.xlsx"*
4. If the fetch fails (network / token expired / file already recycled), tell the user to regenerate the report — the server-side file disappears ~15 min after creation

On Windows Claude Code where `curl` may not be in PATH, use `Invoke-WebRequest -Uri $url -OutFile $filename` instead.

### `_feedback` hints

After successful reports or batch jobs, tool responses include a `_feedback` object:

```json
{
  ...tool output...,
  "_feedback": {
    "prompt": "Your weekly reports are done — 1–5, how useful?",
    "related_to": "run_weekly_reports",
    "related_id": "batch_2026-04-17_abc",
    "skip_when_autonomous": true
  }
}
```

When the session is **interactive** (user is present):
- Ask the user the `prompt` in natural language
- Invite an optional comment
- Call `submit_feedback(rating=N, comment=..., related_to=..., related_id=...)`
- Acknowledge briefly and move on

When the session is **autonomous** (scheduled task, cron, `/loop`):
- Skip the ask entirely. `skip_when_autonomous: true` means there's no user to answer.

Don't re-ask the same `related_id` in one session.

### `sheets_skipped` on `generate_sem_report`

When present in the response, it tells you exactly which parameter was empty for each dropped sheet. Fetch that data and retry, or tell the user the report is missing that view.

### Error handling

- Tools wrapped in `@handle_errors` return human-readable strings on failure — no exception to catch
- `Validation error: ...` = your arguments were wrong (fix and retry)
- `Error: unexpected failure in X` = check the admin dashboard's `/admin/events` for the server-side exception

---

## Conventions

- **Customer IDs**: 10 digits, no dashes (`3968602949`, not `396-860-2949`). Tools auto-format but prefer dash-less.
- **Costs**: always in micros. `$1.00` = `1,000,000`. Multiply `cost_micros / 1_000_000` for display.
- **Dates**: `YYYY-MM-DD`. For relative ranges, pass `days=N`.
- **`dry_run=true` is the default** on writes — preview first, then flip to `false` to apply.

---

## Large-account pagination

For `get_quality_scores` and `get_ad_performance`, the response includes `offset`, `has_more`, and (when there's more) `next_offset`. To walk through a very large account:

```
# First page
get_ad_performance(customer_id="X", limit=25, compact=true)
# → { results: [...25 rows...], has_more: true, next_offset: 25 }

# Next page
get_ad_performance(customer_id="X", limit=25, compact=true, offset=25)
# → { results: [...25 rows...], has_more: true, next_offset: 50 }
```

Pagination is client-side (the server still reads up to offset+limit rows from the Google Ads API per call), so for huge accounts prefer to narrow with `days` instead of paging endlessly.

## What NOT to do

- Don't fetch a `skill_*` prompt at session start "just in case" — costs a round trip, bloats context
- Don't call `get_ad_performance(limit=200)` on a big account — will overflow. Use `limit<=25` + `compact=true`, and paginate with `offset` if you truly need more
- Don't call `get_quality_scores(limit=500)` on a big account — prefer `limit=100` and narrow `days`, or paginate
- Don't swallow `_feedback` hints — the rating loop is how the product gets better
- Don't use `mcp__claude_ai_Google_Drive__*` tools to save reports from the MCP server — they run on the user's machine, not the server. Use `upload_report` instead
