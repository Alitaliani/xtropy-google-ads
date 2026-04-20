# List Scheduled Weekly Reports

Show the user their recurring SEM report schedules — taskId, cadence, next run time, and the accounts each one covers.

## Target

$ARGUMENTS — ignored; always lists all scheduled tasks in the user's Claude Code.

## Workflow

1. Call `mcp__scheduled-tasks__list_scheduled_tasks`.
2. Filter for tasks whose `taskId` or `description` mentions reports (keywords: `weekly-reports`, `sem-reports`, `google-ads`, `xtropy`).
3. For each matching task, present:
   - `taskId`
   - `description` (one-liner)
   - `schedule` (human-readable)
   - `nextRunAt` (ISO → user's local timezone)
   - `lastRunAt` if present
   - `enabled` state

4. If the task was created via `/google-ads:schedule-weekly-reports`, its prompt contains the accounts list + output folder — summarize those for the user.

## Example output

```
Scheduled SEM report runs:

1. weekly-xtropy-reports-my-accounts
   Description: Weekly reports for 4 accounts, saved to ~/Downloads/xtropy-reports/
   Schedule:    Every Monday at 09:00 local time
   Next run:    2026-04-27 09:00 CET
   Last run:    2026-04-20 09:00 CET
   Enabled:     yes
```

## Related commands

- `/google-ads:schedule-weekly-reports` — create a new schedule
- `/google-ads:cancel-weekly-reports` — pause or delete one
