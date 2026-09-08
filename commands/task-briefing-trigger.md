---
description: Manually run the daily task briefing right now instead of waiting for the scheduled morning check
category: thinking
triggers_en: ["task briefing", "run task briefing now", "what's due today", "check my tasks now"]
triggers_es: ["informe de tareas", "revisar mis tareas ahora"]
---

Use the obsidian-second-brain skill. Execute `/task-briefing-trigger $ARGUMENTS`:

This command takes no argument - it reports on the owner's own task board, not on a specific person. It's the on-demand version of the `daily-task-briefing` scheduled task: instead of waiting for its own scheduled morning run, execute its exact read-and-notify logic right now.

1. Resolve today's actual date first - every bucket below depends on it.
2. Read `Boards/Personal.md` in full - a Kanban board (kanban-plugin format) with columns as `## <emoji> <Column Name>` headers (e.g. Backlog, In Progress, This Week, Waiting On, Done, Recurring). Parse every task line under every column EXCEPT the Done and Recurring columns (recurring habits track a cadence, not a fixed due date, and are out of scope here). Each task line has this shape:
   `- [ ] <priority-emoji> **<Title>** · due @{YYYY-MM-DD}`
   possibly followed by an indented description line containing a wikilink to the task's own file. Extract the title, the due date, the priority emoji (whatever the board line actually shows - never invent one), and the wikilink if present. Skip any line with no `due @{...}` date entirely - it has nothing to bucket into.
3. Bucket every parsed task into exactly one of:
   - TODAY: due date equals today
   - THIS WEEK: due date is after today AND on or before the upcoming Sunday (empty if today itself is Sunday, since there's no later day left in the week)
   - OVERDUE: due date is before today
   A due date further out than this week is not shown in any bucket - this briefing is about today, this week, and anything already missed, not the full backlog.
4. Sort each bucket HIGHEST PRIORITY FIRST using each task's own board emoji, highest urgency color first, then lower, then unmarked last - preserve the board's own color exactly, never recolor or reassign a priority.
5. Compose ONE message, in this exact order and emphasis (Today leads and reads as unmissable, since that's the whole point of this command):
   ```
   🗓️ Daily task briefing - <today's date>

   📌 TODAY (<count>)
   <one line per task: "<priority-emoji> <Title>", or "Nothing due today." if empty>

   📅 THIS WEEK (<count>)
   <same format, or "Nothing else due this week." if empty>

   ⏳ OVERDUE (<count>)
   <same format, or "Nothing overdue." if empty>
   ```
   For any task with its own linked file, build an Obsidian deep link (`obsidian://open?vault=<VaultName>&file=<PATH>`, vault-relative path without the `.md` extension, URL-encoded - spaces as `%20`, `/` as `%2F`, emoji folder/column names encoded too) and wrap it as `[<priority-emoji> <Title>](obsidian://...)` so it renders as a clickable link (many chat clients don't auto-linkify custom URI schemes like `obsidian://` on their own).
6. Send the composed message to the vault's configured notification channel (the same one `daily-task-briefing` uses) plus a separate desktop push notification naming just the today-count and overdue-count, e.g. "📌 3 due today, 2 overdue - check Slack for the full list."
7. Report back briefly here in chat too - unlike the scheduled task, this was invoked live, so summarize what was found (the counts per bucket) and confirm it was sent, rather than relying solely on the notification.

HARD RULES:
- Read `Boards/Personal.md` only - never Google Calendar, Google Drive, or any other source for this command.
- Never modify `Boards/Personal.md` or any task file under `Tasks/` - this is read-only reporting, nothing gets checked off, moved, or edited.
- Never include anything from the Done or Recurring columns.
- Even if all three buckets are empty, still send the message and report back saying so - the point is a firm, reliable check, not a silent skip.

This does not replace `daily-task-briefing` - that scheduled task keeps running on its own schedule regardless. This command exists for checking right now instead of waiting for the next scheduled run.

---

This command does not create or update any vault note, so the usual AI-first write rule does not apply to its own output. Its inputs still must be read faithfully - nothing invented beyond what the board itself actually shows.
