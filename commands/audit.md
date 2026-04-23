# Google Ads Account Audit

Run a comprehensive audit of a Google Ads account using MCP tools and the optimization methodology.

## Target Account

$ARGUMENTS — provide a customer ID (e.g., 123-456-7890) or account name.

## Audit Workflow

1. **Discover accounts**: Use `list_accounts` to verify the target account exists and get its customer ID
2. **Check currency**: Use `get_account_currency` to understand cost units before analyzing spend data
3. **Campaign performance**: Run `get_campaign_performance` for the last 30 days — overview of all campaigns
4. **Quality Score analysis**: Run `get_quality_scores` — flag keywords with QS <= 4 that have significant spend (> 10% of budget)
5. **Search terms audit**: Run `get_search_terms` — identify terms with high spend and zero conversions (wasted spend)
6. **Budget pacing**: Run `get_budget_pacing` — check for campaigns over/under budget pacing by > 20%
7. **Competitive position**: Run `get_competitive_position` — flag campaigns with > 30% impression share lost to budget or rank
8. **Change history**: Run `get_change_history` — identify recent changes that explain performance shifts
9. **Ad creative review**: Run `get_ad_creatives` — flag stale ads unchanged for 90+ days with declining CTR
10. **Recommendations**: Run `get_recommendations` — review Google's optimization suggestions

## Analysis Framework

For each step, apply the Account Hygiene Checklist from the optimization skill. Identify:
- Keywords with QS <= 4 and significant spend
- Search terms with high spend and zero conversions
- Campaigns over/under budget pacing by > 20%
- Campaigns with > 30% impression share lost
- Stale ads unchanged for 90+ days

Synthesize findings into prioritized, actionable recommendations with estimated impact.
