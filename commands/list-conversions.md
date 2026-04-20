# List Conversion Actions

List all conversion actions configured for a Google Ads account, with recent conversion counts.

## Target Account

$ARGUMENTS — account name or customer ID (e.g., "Client A" or "1112223333")

## Workflow

1. **Resolve target account** (same pattern as list-campaigns):
   - Call `list_accounts`, then resolve by ID or name via `run_gaql`
2. **Get currency**: Call `get_account_currency` for the account
3. **Fetch ALL configured conversion actions** via `run_gaql`:
   ```
   SELECT conversion_action.name, conversion_action.id,
          conversion_action.type, conversion_action.status,
          conversion_action.category
   FROM conversion_action
   WHERE conversion_action.status = 'ENABLED'
   ORDER BY conversion_action.name
   ```
4. **Fetch recent conversion counts**: Call `get_conversion_action_breakout` with days=30 to get per-action totals
5. **Merge results**: For each configured action, show its 30-day conversion count (0 if no activity in that period)
6. **Display results** as a numbered table:

   | # | Conversion Action | Type | Category | 30-Day Conversions |
   |---|---|---|---|---|
   | 1 | MCC - CARE/FERA - EN - Button Click | WEBPAGE | SUBMIT_LEAD_FORM | 420 |
   | 2 | MCC - ESA - EN - Button Click | WEBPAGE | SUBMIT_LEAD_FORM | 85 |
   | 3 | MCC - SGIP - CTA Button Click | WEBPAGE | SUBMIT_LEAD_FORM | 0 |

7. **Suggest next step**: "To generate a report with specific conversion actions, use `/google-ads:generate-report` and specify the action name patterns to include (e.g., CARE, ESA, LIHEAP)."
