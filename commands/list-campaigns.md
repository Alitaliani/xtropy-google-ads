# List Campaigns

List campaigns for a specific Google Ads account with key metrics.

## Target Account

$ARGUMENTS — account name or customer ID (e.g., "Client A" or "1112223333")

## Workflow

1. **Discover accounts**: Call `list_accounts` to get accessible accounts
2. **Resolve target account**:
   - If $ARGUMENTS looks like a 10-digit number, use it directly as the customer ID
   - Otherwise, query each account's `customer.descriptive_name` via `run_gaql` and match against $ARGUMENTS (case-insensitive substring match)
   - If multiple accounts match, show them and ask the user to pick one
   - If no match, show all available accounts and ask the user to clarify
3. **Get currency**: Call `get_account_currency` for the resolved account
4. **Fetch campaigns**: Call `get_campaign_performance` with the resolved customer_id and days=30
5. **Display results** as a table sorted by cost descending:

   | Campaign | Status | Type | Impr | Clicks | CTR | Cost | Conv |
   |---|---|---|---|---|---|---|---|

Note: Only shows campaigns with activity in the last 30 days.

After displaying, suggest next steps:
- "Use `/google-ads:list-conversions <account>` to see conversion actions for this account"
- "Use `/google-ads:generate-report <account> <dates> <filter>` to generate a filtered report"
