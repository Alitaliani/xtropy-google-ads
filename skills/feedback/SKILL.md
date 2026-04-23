---
name: google-ads-feedback
description: How to capture user feedback after meaningful Google Ads MCP outputs so the system compounds quality over time
---

# Feedback loop

Some tool responses include a `_feedback` object. When you see one, the server is asking you to collect a quick rating from the user so the team can monitor adoption and quality.

## The convention

A response like this:

```json
{
  "status": "completed",
  "results": [...],
  "_feedback": {
    "prompt": "Weekly reports done — 40 succeeded. On 1–5, how useful was this batch?",
    "related_to": "run_weekly_reports",
    "related_id": "batch_2026-04-17_a1b2c3d4",
    "skip_when_autonomous": true
  }
}
```

means: after presenting the tool result to the user, ask them the `prompt`, then record their answer with the `submit_feedback` tool.

## What to do

1. Finish presenting the tool result to the user normally. Do not skip or reorder — feedback is the last thing that happens.

2. **If the session is interactive** (the user is present and can answer):
   - Ask the user, in natural language, using the text in `_feedback.prompt`
   - Invite an optional comment: "Any comment?" — don't push if they're curt
   - When the user answers, call `submit_feedback` with:
     - `rating` = the integer 1–5 they gave (map natural language: "great" → 5, "useless" → 1)
     - `comment` = their free-form comment, or empty string if none
     - `related_to` = the value from `_feedback.related_to`
     - `related_id` = the value from `_feedback.related_id`
   - Briefly acknowledge ("thanks, recorded") and move on

3. **If the session is autonomous** (a scheduled task, cron job, or other non-interactive invocation) AND `_feedback.skip_when_autonomous` is `true`:
   - Do NOT ask for feedback. There is no one to answer.
   - Do NOT call `submit_feedback` with a made-up rating.
   - Simply proceed with the rest of the task.

4. **Deduplicate per session.** If you already asked about this specific `related_id` in this session, do not ask again. The server also deduplicates on its side (24h window per user+id), so a duplicate call is harmless but still wasteful.

## Recognizing an autonomous context

You're in an autonomous context when:
- You were invoked by a scheduled task / cron job (no live user prompt preceded this work)
- The session started from a CronCreate, ScheduleWakeup, or a `/loop` dynamic invocation
- The most recent user turn is absent or was long ago

If you're unsure whether the session is interactive or autonomous, **skip the feedback ask**. Over-asking is worse than missing one.

## What not to do

- Don't ask after every tool call — only when `_feedback` is present
- Don't invent ratings the user didn't give
- Don't save partial feedback — if the user declines or ignores, just move on
- Don't re-prompt or nag if they change the subject
- Don't show the raw `_feedback` JSON to the user — translate the prompt into natural speech

## Why this matters

Every rating is a signal. Low-rated reports reveal which accounts or report modes need tuning. High-rated audits confirm where the methodology is landing. Over weeks, the admin reviews feedback and turns it into concrete improvements (tool fixes, skill refinements, new tests). That feedback loop is how the system gets sharper. Without it, usage metrics only tell us *how much* the system is used — not whether it's any good.

Users can ignore the ask; that's fine. A 30% response rate still produces enough signal to act on weekly.
