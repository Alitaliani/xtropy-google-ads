# List Google Ads Accounts

List all accessible Google Ads accounts with their names, IDs, and status.

## Workflow

1. **Get account IDs**: Call `list_accounts` to get all accessible customer IDs
2. **Resolve names**: For each account, call `run_gaql` with:
   ```
   SELECT customer.descriptive_name, customer.id, customer.status FROM customer
   ```
3. **Display results** as a table:

   | Account Name | Customer ID | Status |
   |---|---|---|
   | Client A - Search | 111-222-3333 | ENABLED |
   | Client B - PMax | 444-555-6666 | ENABLED |

If no accounts are accessible, inform the user to check credentials with `/google-ads:setup`.

After displaying, suggest next steps:
- "Use `/google-ads:list-campaigns <account>` to see campaigns for a specific account"
- "Use `/google-ads:list-conversions <account>` to see conversion actions"
