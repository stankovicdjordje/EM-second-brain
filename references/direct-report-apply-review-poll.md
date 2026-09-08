# Scheduled task: direct-report-apply-review-poll

This is the other half of the automation loop, alongside `direct-report-transcript-poll.md`. Where that task writes review notes, this one watches for the ones you've actually reviewed and turns them into real vault changes - unattended, on a timer, with no live chat exchange required.

The gate is a single checkbox. A review note sits untouched in `Transcripts/To Review/` until its master `## Review status` checkbox is checked (whether you check it manually in Obsidian, or in a live `/direct-report-apply-review` chat). This task's whole job is to notice when that happens and apply exactly what else got checked.

## Setting up the trigger

Register this as a scheduled task the same way as `direct-report-transcript-poll`, in whatever local task-scheduling tool your setup provides. It needs:

- **A recurring schedule**, independent of the transcript-poll task's own schedule. Hourly works well, e.g. cron `37 * * * *` (37 minutes past every hour, every day) - offset from the transcript poll's own minute so they don't race each other, and there's no reason to restrict this one to working hours, since it's just checking for checked boxes, not generating new content.
- **The same local tool access** as the transcript-poll task: your vault's filesystem and your notification channel(s).
- **The prompt below**, filled in with your own values.

## The task prompt

Fill in every `<PLACEHOLDER>` before registering this, then use the rest verbatim as the scheduled task's instructions:

```
You are running an unattended, self-contained check. You have no memory of any prior conversation - everything you need is below.

VAULT ROOT: <path to your vault>
OWNER: <Owner Name> (<owner email>)
NOTIFICATION CHANNEL: <e.g. a Slack channel ID, private, owner-only - post confirmations here, NOT as a direct message>

GOAL: once a run, find every review note sitting in ANY direct report's "Transcripts/To Review/" subfolder (never their "Transcripts/Done/", and never any "Transcripts/_state.md" file - those are siblings, not sources) whose master "reviewed" checkbox has been checked. For each one found, apply whichever itemized boxes are also checked, move the note to that same person's "Transcripts/Done/", and confirm to the owner. Notes whose master checkbox is still unchecked are left alone entirely - no message, no action, they simply wait for the next run.

STEPS:

1. Find every file matching the pattern "Knowledge Base/People/**/Transcripts/To Review/*.md" in the vault (a recursive search under Knowledge Base/People/ for any .md file inside a "Transcripts/To Review/" folder, at any depth/team-subfolder). Use this recursive approach rather than enumerating a fixed list of people by name - that way a newly added direct report's folder is picked up automatically with no update needed to this task. If nothing matches, stop - nothing to do this run.
2. For each matching file:
   a. Read it. Find its "## Review status" section and check the single checkbox there.
   b. If it reads "- [ ] I have reviewed this note and it's ready to apply" (still unchecked), skip this file entirely - leave it exactly as-is, do not touch it, do not message about it.
   c. If it reads "- [x] I have reviewed this note and it's ready to apply" (checked), this file is ready to process - continue to step 3 for this file.
3. The filename is "<YYYY-MM-DD> - <Person Full Name>.md" (date first) and the file's own path tells you which person's folder it's in (the "Transcripts/To Review/" parent's grandparent folder is that person's own folder). Follow this repo's `commands/direct-report-apply-review.md` exactly - read that file now and apply its steps 4-9 to this specific note (you already know the master box is checked, so proceed straight to parsing the itemized checkboxes and applying them - do not re-check that command's own master-checkbox step, you've already done the equivalent here). For its "move" step, move the note to that same person's own "Transcripts/Done/" (not a shared location). For its notification step, post to your configured notification channel directly - do NOT search for or DM the owner's user profile if your channel is a group/team channel.
4. Move on to the next matching file and repeat from step 2. Process every qualifying file found this run, not just the first one - with more direct reports, more than one may be ready in the same run.

HARD RULES:
- Never look inside anyone's "Transcripts/Done/" for anything to process - it's a destination, not a source.
- Never touch any "Transcripts/_state.md" file - those belong to the other scheduled task (direct-report-transcript-poll).
- Never apply anything from a note whose master checkbox is unchecked.
- Never touch Performance Review or AI Competency Framework files.
- If no notes anywhere have their master checkbox checked, do nothing at all this run - no notes, no notifications. This is expected and fine most runs.
```

## Placeholders to fill in

| Placeholder | What it is |
|---|---|
| `<path to your vault>` | Absolute filesystem path to your Obsidian vault |
| `<Owner Name>` / `<owner email>` | You |
| `<e.g. a Slack channel ID...>` | Wherever you want confirmations delivered - keep it private/owner-only |

## Why two separate tasks instead of one

Keeping the scan (find new transcripts, propose changes) and the apply (read back what got checked, commit it) as two independently-scheduled tasks means:

- The scan can run only during your working hours, since new transcripts only show up then; the apply can run around the clock, since it's cheap and just checking for checked boxes.
- A slow or interrupted review doesn't block new transcripts from being detected - they're entirely decoupled.
- Either one can be paused, edited, or re-scheduled without touching the other.
