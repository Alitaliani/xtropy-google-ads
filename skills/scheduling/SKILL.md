---
name: scheduling
description: How to help users set up, list, and cancel recurring Google Ads report schedules that download to local folders (no email, no Drive).
---

# Recurring SEM Report Scheduling (End-User Model)

When a user asks to "schedule a weekly report", "run this every Monday", "set up automated reports", or anything similar, use this methodology.

## Core model

- Reports run on a **cron schedule inside the user's own Claude Code / Cowork session**. They do NOT run on Hamid's server. The user's Claude Code must be open (or at least launched periodically) for the cron to fire.
- When the schedule fires, Claude calls `mcp__google-ads-cloud__run_weekly_reports` with `skip_drive_upload=true` and no `summary_email`. The tool returns tokenized download URLs per account.
- Claude then downloads each `.xlsx` to the folder the user chose at schedule-creation time. No Drive upload. No email.

## When to use `/google-ads:schedule-weekly-reports`

Invoke when the user's intent is clearly recurring automated reports. Phrases like:
- "Schedule this every Monday"
- "Every month on the 1st, generate…"
- "Set up automated weekly audits"
- "Can this run on a cron?"

Do NOT invoke for one-off report requests — those use `generate_sem_report` directly (which already auto-downloads).

## Required inputs

Before creating any scheduled task, gather:

1. **Which accounts** — explicit list of customer_ids. Use `list_accounts` to help the user pick.
2. **Output folder** — absolute path where Claude will save downloaded reports. Default to user's current working directory, but confirm.
3. **Cadence** — weekly / monthly / quarterly.
4. **Time of day** — local time. Default Monday 09:00 for weekly.

Prefer `AskUserQuestion` for these — multiple choice where natural (cadence, day-of-week), free text for the folder path.

## Disallowed behaviors

- **Never email end users.** The `summary_email` param must be empty for end-user schedules. Only the operator cron sends email.
- **Never upload to Drive.** Always pass `skip_drive_upload=true` for end-user schedules.
- **Never hardcode operator emails** (`team@xtropy.net`, `jim@xtropy.net`, `hamid@trafficarchitect.com`) in any schedule created for an end user.
- **Never default `accounts=""`** — always pass an explicit list of customer_ids. Empty accounts falls back to the MCC hierarchy which can span beyond the user's intended scope.

## Scheduled-task prompt template

```text
Autonomous scheduled run. User not present.

1. Call mcp__google-ads-cloud__run_weekly_reports with:
   - accounts='[<json-list-of-cids>]'
   - cadence="<weekly|monthly|quarterly>"
   - skip_inactive=true
   - summary_email=""
   - skip_drive_upload=true
   - test_mode=false

2. For each result in the response where status == "success" and _auto_download
   is present: fetch the URL (token is embedded — no auth header) and save as
   <OUTPUT_FOLDER>/<filename>.

3. Report the tally: succeeded / failed / skipped_inactive / excluded, plus
   the absolute path to each saved file.

Do NOT email, do NOT upload to Drive. If any download URL is expired (15-min
TTL), log it and continue — do not retry the whole batch.
```

## Cron expressions (LOCAL time)

- weekly: `<MIN> <HOUR> * * <DOW>` — DOW is 0=Sun..6=Sat (or 1=Mon..0=Sun depending on locale; verify)
- monthly: `<MIN> <HOUR> 1 * *`
- quarterly: `<MIN> <HOUR> 1 1,4,7,10 *`

Use the `scheduled-tasks` MCP server's `create_scheduled_task` with `cronExpression` (not `fireAt`) for recurring.

## Download TTL gotcha

The tokenized download URLs expire 15 minutes after mint. If the user's Claude Code isn't running when the cron fires and only opens hours later, the URLs will 401. The scheduled-task runner should:
- Log which accounts produced dead tokens.
- Continue processing remaining accounts.
- Surface the failures in the final summary so the user can manually re-run via `/schedule → run now` on that task.

Do NOT retry the whole batch automatically — that would duplicate generation work for accounts that already downloaded successfully.

## Related skills

- `reporting/SKILL.md` — report generation methodology (single-account)
- `optimization/SKILL.md` — what the reports tell us

## Operator vs end-user split

The operator (Xtropy's own weekly SEM run) uses a **local stdio** MCP with `skip_drive_upload=false` + `summary_email="team@xtropy.net"` — it DOES upload to Drive and email the team. That path is operator-only and lives in `~/.claude/scheduled-tasks/weekly-sem-reports/SKILL.md` on the operator's machine. End users never hit that path.
