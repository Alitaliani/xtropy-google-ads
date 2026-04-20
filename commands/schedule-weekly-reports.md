# Schedule Weekly Reports

Set up a recurring weekly (or monthly/quarterly) SEM report run for a set of Google Ads accounts you specify. When the schedule fires, reports are generated server-side and downloaded to the local folder you choose. No Drive upload, no email.

## Target

$ARGUMENTS — free text. If empty, Claude walks the user through configuration.

## Workflow

### 1. Confirm which accounts the user wants scheduled

- Call `list_accounts` to show available MCC client accounts with names + IDs.
- Ask the user (via `AskUserQuestion` when available) for:
  - **Which accounts** to include — multi-select, or "all active".
  - **Output folder** on their machine (absolute path). Default: current working directory.
  - **Cadence** — weekly / monthly / quarterly. Default: weekly.
  - **Local time to fire** — default Monday 09:00 user-local for weekly.

Never assume recipient email — this command does NOT email anyone. Delivery is local download only.

### 2. Build the scheduled-task prompt

The prompt that will run every cycle must be completely self-contained. Bake in the user's choices directly so no lookups happen at fire time.

```
Autonomous scheduled run. User not present.

1. Call mcp__google-ads-cloud__run_weekly_reports with:
   - accounts='["<cid1>","<cid2>",...]'
   - cadence="<weekly|monthly|quarterly>"
   - skip_inactive=true
   - summary_email=""
   - skip_drive_upload=true
   - test_mode=false

2. For each result in the response where status == "success" and
   _auto_download is present: fetch the URL (no auth header needed, token
   is embedded) and save the file as <OUTPUT_FOLDER>/<filename>.

3. Report the final tally: succeeded / failed / skipped_inactive / excluded,
   and the absolute path to each saved file.

Do NOT email anyone. Do NOT upload anywhere. If any download URL has expired
(15-minute TTL), log the expired filename and continue — do not retry the
whole batch.
```

### 3. Create the scheduled task

Use `mcp__scheduled-tasks__create_scheduled_task` with:
- `taskId`: kebab-case derived from the user's description (e.g. `weekly-xtropy-reports-my-accounts`)
- `description`: one-liner summarizing which accounts + cadence + output folder.
- `cronExpression`: built from the user's chosen cadence + day/time (LOCAL time):
  - weekly: `M H * * D` where D is the day-of-week index (Mon=1..Sun=0)
  - monthly: `M H 1 * *`
  - quarterly: `M H 1 1,4,7,10 *`
- `prompt`: the prompt from step 2.
- `notifyOnCompletion`: true.

### 4. Confirm + test

After creation, offer to fire a one-off "test run" immediately (using `fireAt`) so the user knows the full path works before waiting for the first cron tick.

## Gotchas

- **Expired download tokens**: the auto-download URLs expire 15 minutes after mint. If the user's Claude Code isn't running at the scheduled time and the session doesn't open until hours later, the tokens will be dead. Safe to re-run the schedule manually from `/schedule` → "run now".
- **Output folder**: must be an absolute path the user's OS accepts. On Windows prefer `C:\Users\<name>\...` over `~/...` for reliability in autonomous sessions.
- **Account scope**: if the user says "all active accounts", explicitly enumerate customer_ids rather than passing `accounts=""`. Empty-accounts relies on the server's MCC hierarchy which may include accounts this user shouldn't see.

## Related commands

- `/google-ads:list-weekly-reports` — see what you have scheduled
- `/google-ads:cancel-weekly-reports` — pause or delete a schedule
