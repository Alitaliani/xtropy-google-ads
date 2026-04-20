# Schedule SEM Report

Set up a recurring SEM performance audit report that runs automatically.

## Target Config

$ARGUMENTS — provide a config name (e.g., "care_weekly") to schedule, or "new" to create a config first.

## Important Limitations

- Scheduled jobs only run **while this Claude session is active** — they do not persist across sessions
- Recurring jobs **auto-expire after 3 days** — re-schedule when starting a new session
- Reports are saved to the configured output directory on the local filesystem

## Scheduling Workflow

### If scheduling an existing config:

1. **Load config**: Use `get_report_config` to load the saved configuration
2. **Review settings**: Show the user the config summary (account, cadence, sheets, date range)
3. **Compute date range**: The config includes the next date range that would be used
4. **Schedule with CronCreate**: Set up the recurring job using the cadence's cron expression:
   - Weekly: `17 9 * * 1` (Monday 9:17am)
   - Monthly: `23 8 1 * *` (1st of month 8:23am)
   - Quarterly: `37 8 1 1,4,7,10 *` (1st of Jan/Apr/Jul/Oct 8:37am)
5. **Confirm**: Display the schedule, next run time, and 3-day expiry notice

The CronCreate prompt should be:

```
Run the SEM performance audit report for {account_name} ({customer_id}) using saved config "{config_name}".

Steps:
1. Fetch data using get_campaign_performance, get_adgroup_performance, etc. for the {cadence} period
2. Call generate_sem_report with the fetched data to produce the Excel report
3. Call upload_report with the file_path from step 2 to upload to Google Drive, restrict access, and send Slack + Gmail notifications

Use account_name="{account_name}", report_period="{date_range}", and the sheet_count from the generate_sem_report response.
```

### If creating a new config:

1. **Gather info**: Ask the user for:
   - Customer ID or account name
   - Cadence: weekly, monthly, quarterly
   - Which sheets to include (default: all)
   - Output directory (default: reports/{config_name})
2. **Save config**: Use `save_report_config` to persist the configuration
3. **Schedule**: Follow the scheduling workflow above

### Managing schedules:

- **List active jobs**: Use `CronList` to see all active scheduled reports
- **Cancel a job**: Use `CronDelete` with the job ID to stop a scheduled report
- **List configs**: Use `list_report_configs` to see all saved configurations
- **Delete a config**: Use `delete_report_config` to remove a saved configuration

## Cadence Date Range Logic

| Cadence | Date Range Computed | Example |
|---|---|---|
| Weekly | Last 7 days (yesterday back 7 days) | 3/3/26-3/9/26 |
| Monthly | Last full calendar month | 2/1/26-2/28/26 |
| Quarterly | Last full calendar quarter | 10/1/25-12/31/25 |
| YTD | January 1 to today | 1/1/26-3/15/26 |
| Flight | Flight start to today | 1/19/26-3/15/26 |

## Example Usage

```
/google-ads:schedule-report care_weekly
/google-ads:schedule-report new
/google-ads:schedule-report list
```
