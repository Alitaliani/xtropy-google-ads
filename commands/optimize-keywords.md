# Keyword Optimization

Audit search terms for wasted spend and irrelevant traffic, then add negative keywords to clean up campaigns.

## Target Account

$ARGUMENTS — account name or customer ID, plus optional campaign filter (e.g., "Client A" or "1112223333 CARE campaigns only")

## Workflow

1. **Resolve account**: Call `list_accounts` and resolve to customer_id
2. **Get currency**: Call `get_account_currency`
3. **Fetch search terms**: Call `get_search_terms` with days=30, limit=500
4. **Fetch campaign performance**: Call `get_campaign_performance` for context on budget allocation
5. **Fetch existing negatives**: Call `list_negative_keywords` for each campaign to avoid duplicates

6. **Analyze search terms** using the optimization skill methodology:

   **Wasted Spend** — terms with high cost and zero conversions:
   | Search Term | Campaign | Impr | Clicks | Cost | Conv | Action |
   |---|---|---|---|---|---|---|

   **Irrelevant Terms** — terms that don't match campaign intent:
   - Flag terms unrelated to the product/service
   - Flag competitor brand terms (unless intentional)
   - Flag overly broad terms diluting Quality Score

   **High Performers** — terms with strong conversion rates that should be added as exact match keywords:
   | Search Term | Campaign | Conv Rate | Cost/Conv | Recommendation |
   |---|---|---|---|---|

7. **Recommend negative keywords**: Group into:
   - **Campaign-level negatives**: irrelevant to a specific campaign
   - **Account-level negatives**: irrelevant to all campaigns
   - Present with match type recommendations (exact, phrase, broad)

8. **Apply negatives** (with user confirmation):
   - Show the proposed negative keyword list
   - Ask user to confirm before adding
   - Call `add_negative_keywords` with `dry_run=true` first to validate
   - If user approves, call again with `dry_run=false` to apply
   - Report results: how many added, estimated spend saved

## Safety

- Always run `dry_run=true` first and show results before applying
- Never add negatives that would block converting search terms
- Flag any negative that would conflict with existing keywords
