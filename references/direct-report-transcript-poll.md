# Scheduled task: direct-report-transcript-poll

This is the unattended companion to `/direct-report`. Instead of you invoking the command live, a scheduled task runs on a timer, checks each direct report for a new 1-on-1 transcript, and - if it finds one - writes the same kind of proposal checklist that `/direct-report` would produce live, then notifies you instead of waiting for a reply.

It never applies anything by itself. It only ever produces a review note under that person's own `Transcripts/To Review/` subfolder and sends a notification. See `direct-report-apply-review-poll.md` for the other half of the loop, which actually applies what you check off.

## Setting up the trigger

Register this as a scheduled task in whatever local task-scheduling tool your setup provides (for example, a `scheduled-tasks` MCP server with a `create_scheduled_task`-style tool). It needs:

- **A recurring schedule.** A sensible default is hourly during your working hours on workdays, e.g. cron `7 7-18 * * 1-5` (7 minutes past the hour, 7am-7pm, Monday-Friday) - adjust the hours/days to your own working pattern. There's no need to run it overnight or on weekends if 1-on-1s only happen during the week.
- **Local tool access** to your vault's filesystem, your transcript source (e.g. a Google Drive MCP connector), and whatever channel you use for notifications (Slack MCP, a push-notification tool, etc.). A cloud-only or sandboxed scheduler that can't reach your local vault won't work for this - it needs to actually read and write vault files.
- **The prompt below**, filled in with your own values (see the placeholder table).

## The task prompt

Fill in every `<PLACEHOLDER>` before registering this, then use the rest verbatim as the scheduled task's instructions:

```
You are running an unattended, self-contained check. You have no memory of any prior conversation - everything you need is below.

VAULT ROOT: <path to your vault>
VAULT NAME (for Obsidian URIs): <VaultName>
OWNER: <Owner Name> (<owner email>)
NOTIFICATION CHANNEL: <e.g. a Slack channel ID, private, owner-only - post notifications here, NOT as a direct message>

GOAL: check whether any of the owner's direct reports has a new 1-on-1 transcript since it was last checked, and if so, produce a review checklist note in that person's own "Transcripts/To Review/" subfolder (never apply anything automatically) and notify the owner, including a one-click Obsidian deep link to the note.

PEOPLE (name -> vault folder, containing that person's Profile Card/Skills Matrix/etc. plus a "Transcripts/" subfolder):
- <Person> -> "Knowledge Base/People/<Team>/<Person> (<Role>)/"
(This list will grow as more direct reports are added - if the vault's own _CLAUDE.md or Knowledge Base/People/ folder structure indicates additional people with a "Transcripts/" subfolder already set up, include them too rather than treating this list as exhaustive forever.)

STATE FILE: each person's own "<their folder>/Transcripts/_state.md" (sibling to "To Review/" and "Done/" inside their "Transcripts/" folder - never scan or move it as if it were a review note). If it doesn't exist for a person, create it (frontmatter matching the vault's ai-first rules, then a single line: "- Last processed doc id = <id>, date = <date>"). Read it first, per person, to know what's already been handled.

STEPS, for EACH person in turn:

1. Search your transcript source for the person's most recent 1-on-1 meeting document (adapt the search to whatever naming convention your transcripts actually use). Sort by created time, take the newest.
2. Skip this person entirely (no note, no notification, no state change) if:
   - no such document exists, OR
   - its document ID matches what's already recorded as "last processed" in that person's own Transcripts/_state.md, OR
   - it was created less than 60 minutes ago (too fresh - some transcription tools' notes may still be settling).
3. Otherwise, this is a new transcript to process:
   a. Read its full content. Note its URL and title - you'll need both for step e.
   b. Read that person's existing "<Person> - Profile Card.md" and "<Person> - Skills Matrix.md" in their vault folder above, in full.
   c. Read the vault's "Templates/Profile Card Template.md" and "Templates/Skills Matrix Template.md" for the current expected structure.
   d. For the exact diffing/formatting logic (what counts as new/resolved content, what to exclude, what "template conformance" drift looks like, and the checkbox output format), follow the same approach as this repo's `commands/direct-report.md` - read that file now and apply its steps 4-6, EXCEPT do not present the output live and do not wait for confirmation (there is no one to respond) - instead do the following:
   e. Write a new note at vault-relative path "<that person's folder>/Transcripts/To Review/<YYYY-MM-DD> - <Person Full Name>.md" (today's date FIRST in the filename, so files sort chronologically), following this vault's ai-first rules - a "## For future Claude" preamble, proper frontmatter (type: log, date, tags, ai-first: true), then a "**Source transcript:** [<doc title>](<url>)" line so the owner can open the raw transcript themselves and add anything the automated diff missed, then a "## Review status" section containing EXACTLY one checkbox: "- [ ] I have reviewed this note and it's ready to apply" (this is the master gate - nothing gets applied until this specific box is checked), then the same four-heading unchecked itemized checkbox list `direct-report.md` produces: "## Template Conformance", "## Profile Card", "## Skills Matrix", "## Tasks & Reminders" (omit any of these four with nothing to flag). State clearly near the top that nothing has been applied to any file yet, and that the owner is welcome to add their own extra checkbox items under any of the four headings before checking the master box - `/direct-report-apply-review` and the apply-review-poll scheduled task will pick up anything checked there too, not just what the automation itself proposed. While building this note, COUNT how many itemized checkbox items ended up under each of the four headings (0 for any heading you omitted) - you need these counts for step h.
   f. Update "<that person's folder>/Transcripts/_state.md" with this document's ID and date.
   g. Build an Obsidian deep link to the new note: `obsidian://open?vault=<VaultName>&file=<PATH>`, where `<PATH>` is the note's vault-relative path WITHOUT the `.md` extension, URL-encoded (spaces as `%20`, `/` as `%2F`, and encode any non-ASCII folder-name characters too).
   h. Notify the owner. Two channels work well together: a chat/messaging notification (formatted as a bulleted breakdown using the counts from step e, with the Obsidian deep link wrapped in explicit markdown link syntax - many chat clients don't auto-linkify custom URI schemes like `obsidian://`) plus a separate push notification, since a message posted under the owner's own identity in some tools (e.g. Slack apps posting as the user) may not generate a notification for that same user - a genuinely separate channel avoids that blind spot. Example chat message:
      ```
      1-on-1 review ready for <Person>

      - Template Conformance: <N> item(s) to review   (omit this line entirely if N is 0)
      - Profile Card: <N> item(s) to review            (omit if 0)
      - Skills Matrix: <N> item(s) to review            (omit if 0)
      - Tasks & Reminders: <N> item(s) to review        (omit if 0)

      [Open it directly in Obsidian](obsidian://open?vault=<VaultName>&file=...)
      Or find it in <Person>'s own folder under Transcripts/To Review/

      Review it, check the boxes you want applied, then check "I have reviewed this note" at the top - it'll be picked up and applied automatically.
      ```

HARD RULES:
- Never write to, or modify, any person's actual Profile Card or Skills Matrix file. Never create an actual task or board entry. This job only ever produces a new review note in that person's "Transcripts/To Review/" plus a notification.
- If nothing new is found for a person on a given run, do nothing at all for them - no notes, no notifications. This is expected and fine most runs.
- Respect each person's own stated exclusion policy in their existing files (health/family circumstances, other people's private details) when deciding what from the meeting is worth flagging.
```

## Placeholders to fill in

| Placeholder | What it is |
|---|---|
| `<path to your vault>` | Absolute filesystem path to your Obsidian vault |
| `<VaultName>` | The vault's name as Obsidian knows it (used in `obsidian://` links) |
| `<Owner Name>` / `<owner email>` | You |
| `<e.g. a Slack channel ID...>` | Wherever you want notifications delivered - keep it private/owner-only |
| `<Person>`, `<Team>`, `<Role>` | Each direct report's name and where their folder lives |
