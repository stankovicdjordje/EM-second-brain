---
description: Manually run a direct report's transcript check right now, instead of waiting for the scheduled poll
category: thinking
triggers_en: ["trigger direct report check", "run direct report poll now", "check for new transcript now", "manual transcript check"]
triggers_es: ["activar revisión de reporte directo", "revisar transcripción ahora"]
---

Use the obsidian-second-brain skill. Execute `/direct-report-trigger $ARGUMENTS`:

The argument is a direct report's name - handle typos and partial matches. This command is the on-demand version of the `direct-report-transcript-poll` scheduled task, for a single person: instead of waiting up to an hour for that task's own schedule, run its exact scan-and-notify logic right now.

This is **not** the same as `/direct-report`. `/direct-report` presents its findings live in chat and waits for the owner to say which boxes to check. This command produces the same *unattended-style* output the scheduled task would produce - a review note plus a notification - and does not wait for a reply, because the point is to skip the wait for the next scheduled run, not to skip the review step itself.

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match, confirm if ambiguous). Their `Transcripts/` subfolder sits alongside their `<Person> - Profile Card.md` etc.
2. Read `<their folder>/Transcripts/_state.md` (create it, following the vault's ai-first rules, if it doesn't exist yet) to find the document ID of the last transcript already processed for this person.
3. Search for this person's most recent 1-on-1 transcript (same source and naming convention `/direct-report` step 2 uses). Sort by created time, take the newest.
4. Stop here - report back in chat, write nothing, notify no one - if:
   - no transcript document exists for this person, OR
   - its document ID matches what's already recorded in `_state.md` (nothing new since the last run), OR
   - it was created less than 60 minutes ago (too fresh - some transcription tools' notes may still be settling) - tell the owner it exists but say how much longer to wait.
5. Otherwise, this is new. Process it exactly the way `direct-report-transcript-poll` processes one person: read the transcript in full, read that person's existing `<Person> - Profile Card.md` and `<Person> - Skills Matrix.md`, check both against the vault's `Templates/Profile Card Template.md` / `Templates/Skills Matrix Template.md`, and apply `/direct-report`'s steps 4-6 diffing logic (template conformance, content diff, task/reminder scan).
6. Write a new note at `<that person's folder>/Transcripts/To Review/<YYYY-MM-DD> - <Person Full Name>.md`, following the same structure `direct-report-transcript-poll` produces: a `## For future Claude` preamble, proper frontmatter, a `**Source transcript:**` link, a `## Review status` section with exactly one master checkbox (`- [ ] I have reviewed this note and it's ready to apply`), then the four itemized headings (`## Template Conformance`, `## Profile Card`, `## Skills Matrix`, `## Tasks & Reminders`, omitting any with nothing to flag).
7. Update `<that person's folder>/Transcripts/_state.md` with this document's ID and today's date.
8. Notify the owner exactly as `direct-report-transcript-poll` does: a bulleted breakdown (counts per section) posted to the vault's configured direct-report notification channel, plus a separate desktop push notification, both including the Obsidian deep link to the new note.
9. Report back briefly in chat what was found and where the note was written - unlike the scheduled task, this was invoked live, so say so here too rather than relying solely on the notification.

HARD RULES (same as `direct-report-transcript-poll`):
- Never write to, or modify, any person's actual Profile Card or Skills Matrix file. Never create an actual task or board entry directly. This command only ever produces a new review note in that person's `Transcripts/To Review/` plus a notification.
- Respect each person's own stated exclusion policy in their existing files (health/family circumstances, other people's private details) when deciding what from the meeting is worth flagging.
- Never touch Performance Review or AI Competency Framework files.

This does not replace `direct-report-transcript-poll` - that scheduled task keeps running on its own schedule regardless. This command exists for the times the owner doesn't want to wait for the next scheduled run - e.g., running `/direct-report-trigger <name>` right after a 1-on-1 to get the review note and notification immediately instead of within the hour.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
