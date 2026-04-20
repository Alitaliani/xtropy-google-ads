# Cancel or Pause a Scheduled Weekly Report

Pause (disable) or delete a recurring SEM report schedule.

## Target

$ARGUMENTS — taskId to cancel, or free text like "pause the one for PG&E". If empty, list first.

## Workflow

1. If no taskId provided, call `mcp__scheduled-tasks__list_scheduled_tasks` and show the user their schedules; ask which one to act on (via `AskUserQuestion`).
2. Ask whether to **pause** (disable but keep definition) or **delete** (remove entirely).
3. Execute:
   - **Pause**: `mcp__scheduled-tasks__update_scheduled_task(taskId=<id>, enabled=false)`
   - **Delete**: `mcp__scheduled-tasks__delete_scheduled_task(taskId=<id>)`
4. Confirm to the user, showing the task's `description` and final state.

## Notes

- Pausing is reversible: `/google-ads:schedule-weekly-reports` has no "resume" path — use `mcp__scheduled-tasks__update_scheduled_task(taskId=<id>, enabled=true)` instead.
- Deleting cannot be undone. The task's generated reports on disk are not affected.

## Related commands

- `/google-ads:schedule-weekly-reports` — create a new schedule
- `/google-ads:list-weekly-reports` — see what you have scheduled
