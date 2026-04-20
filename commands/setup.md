# Google Ads Plugin Setup Wizard

Interactive guided setup for the Google Ads MCP plugin. Configures credentials, discovers accounts, and enables the report automation pipeline.

## Setup Tiers

| Tier | Name | What it enables |
|------|------|-----------------|
| 1 | **Query** | All 38+ read tools, ad hoc reporting, keyword research |
| 2 | **Reports** | Tier 1 + Excel report generation saved locally |
| 3 | **Full Pipeline** | Tier 2 + Drive upload, archiving, email notifications, scheduling |

## Wizard Flow

### Step 1: Check Prerequisites

Call `check_prerequisites` to verify the system environment.

**Report:**
- Python version (must be >= 3.11)
- rclone availability (optional, for Tier 3)
- Installed optional packages (openpyxl for reports, etc.)
- Existing .env and accounts.json files

If an existing `.env` is detected, ask the user:
> "I found an existing configuration. Would you like to update specific settings, or start fresh?"

### Step 2: Google Ads API Credentials (Required — Tier 1)

Ask the user for:
1. **Service account JSON path** — "Where is your Google Ads service account key file? (full path)"
2. **Developer token** — "What is your Google Ads API developer token?"
3. **MCC ID** — "What is your Manager (MCC) account ID? (10 digits, no dashes)"

After collecting all three, call `validate_credentials` with `scope="google_ads"` to test the connection.

- **If valid**: Show success, display the service account email, proceed to Step 3.
- **If invalid**: Show the error and hint. Let the user correct the values and retry.

### Step 3: Discover Accounts (Tier 2)

Call `discover_accounts` to list all accounts under the MCC with their currencies.

Show the user a summary:
> "Found {count} accounts under your MCC. Here are the first 10:"
> (list account names and IDs)

Ask: "Should I save these to accounts.json? (existing manual edits like GA4 property IDs will be preserved)"

If yes, call `write_accounts_json` with the MCC ID and discovered accounts.

### Step 4: Gmail / Drive Notifications (Optional — Tier 3)

Ask: "Would you like to set up email notifications and Google Drive report delivery? (This enables the full automated pipeline.)"

If yes:
1. Ask for the **Gmail service account JSON path**. If the same file as Step 2, reuse it.
2. Ask for the **sender email** (must be a Google Workspace email).
3. Call `validate_credentials` with `scope="gmail"` to test Gmail API access.
   - **If 403/unauthorized**: Display domain-wide delegation instructions:
     > "Gmail API requires domain-wide delegation. Your Google Workspace admin needs to:
     > 1. Go to Admin Console > Security > API Controls > Domain-wide Delegation
     > 2. Add the service account client ID
     > 3. Authorize scope: https://www.googleapis.com/auth/gmail.send
     > 4. Also authorize: https://www.googleapis.com/auth/drive"
   - Offer to proceed without notifications (drops to Tier 2).
4. Ask for **notification recipients**:
   - Default recipient email
   - Drive share recipients (comma-separated, supports `email:role` format)
   - Slack webhook URL (optional)
5. Ask for **Google Drive folder ID** for report uploads:
   - "Paste the Google Drive folder URL or ID where reports should be uploaded"
   - Extract the folder ID from the URL if a full URL is provided

### Step 5: Write Configuration

Collect all values into a config dict and call `write_env_file` to atomically write `.env`.

The config should include:
```
GOOGLE_ADS_AUTH_TYPE=service_account
GOOGLE_ADS_CREDENTIALS_PATH={path from step 2}
GOOGLE_ADS_DEVELOPER_TOKEN={token from step 2}
GOOGLE_ADS_LOGIN_CUSTOMER_ID={mcc_id from step 2}
GMAIL_CREDENTIALS_PATH={path from step 4, or same as above}
GMAIL_SENDER={email from step 4}
GMAIL_RECIPIENT={recipient from step 4}
DRIVE_SHARE_RECIPIENTS={recipients from step 4}
GDRIVE_FOLDER_ID={folder_id from step 4}
SLACK_WEBHOOK_URL={webhook from step 4}
```

Only include keys for which the user provided values.

### Step 6: Summary

Call `check_setup_status` and present the results:

> **Setup Complete — Tier {N}: {Name}**
>
> Connections:
> - Google Ads API: {status}
> - Gmail API: {status}
> - Google Drive: {status}
> - rclone: {status}
>
> Accounts: {count} configured
> Report modules: {loaded/not loaded}
>
> **Next steps:**
> - Restart Claude Code for settings to take effect
> - Try `/google-ads:list-accounts` to verify
> - Try `/google-ads:sem-report` to generate your first report
> - Set up `/google-ads:schedule-report` for weekly automation

## Resuming Setup

If the wizard is run again with an existing configuration:
- Skip already-validated steps (detect via `check_setup_status`)
- Show current tier and ask what the user wants to reconfigure
- Allow updating individual settings without full re-setup
