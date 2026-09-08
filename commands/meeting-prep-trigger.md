---
description: Manually run one person's meeting-prep checklist right now, writing it to Obsidian instead of presenting live in chat
category: thinking
triggers_en: ["trigger meeting prep", "run meeting prep now", "generate meeting prep file for"]
triggers_es: ["activar preparacion de reunion", "generar archivo de preparacion de reunion"]
---

Use the obsidian-second-brain skill. Execute `/meeting-prep-trigger $ARGUMENTS`:

The argument is a person's name - handle typos and partial matches, with the same owner's-own-manager special case as `/meeting-prep`. This command is the on-demand, single-person version of the `meeting-prep-daily-trigger` scheduled task: instead of waiting for its daily run (which sweeps every qualifying meeting on the calendar), run its exact data-gathering and note-writing logic right now, for just this person. Unlike `/meeting-prep`, the result goes into Obsidian as a file to review later, not into this chat.

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match, confirm if ambiguous), applying the same owner's-own-manager special case as `/meeting-prep` if relevant.
2. Look up this person's next (or today's) weekly 1-on-1 on the owner's calendar, for the note's `**Meeting:**` line.
3. Search Google Drive for this pairing's 1-on-1 transcripts (same naming convention as `/direct-report`), sort by created time, take up to the last 4. Read all of them in full - this is the main driver, weighted by recency exactly as in `/meeting-prep` step 3.
4. Read that person's Profile Card and Skills Matrix in full, and scan `Tasks/📥 Backlog` and `Boards/Personal.md` for open items tied to them - cross-check against these, don't let them drive the note.
5. Build the same three prioritized checklists (Follow-ups / Growth / Achievements) as `/meeting-prep` step 5, following `Templates/Meeting Prep Template.md`'s exact shape (fall back to `references/meeting-prep-template.md`).
6. Write the note to `<that person's folder>/Meeting Prep/To Review/<YYYY-MM-DD> - <Person Full Name>.md`, matching the template exactly: frontmatter, a `## For future Claude` preamble stating which of the transcripts read were used and how (say so explicitly if fewer than 4 exist - never pad) and that nothing has been sent anywhere yet, the `**Meeting:**` and `**Meetings reviewed**` lines, the `## Review status` master checkbox, then the three checklist headings as unchecked boxes.
7. Notify the owner: a message to the vault's configured meeting-prep notification channel (the same one `meeting-prep-daily-trigger` and `/meeting-prep-apply` use) plus a separate desktop push notification, naming the person and a count per bucket, with an Obsidian deep link to the new note.
8. Report back briefly here in chat what was found and where the note was written - unlike the scheduled task, this was invoked live, so say so here too, even though the note itself is the main deliverable.

HARD RULES:
- Never write to, or modify, any person's actual Profile Card, Skills Matrix, Performance Review, or AI Competency Framework file - this only ever produces a new note under `Meeting Prep/To Review/` plus a notification.
- Never fabricate a meeting, a talking point, or a person's vault folder. If something can't be found, say so explicitly rather than guessing.
- Respect each person's own stated exclusion policy (health/family circumstances, other people's private details).

This does not replace `meeting-prep-daily-trigger` - that scheduled task (once enabled) keeps sweeping every qualifying meeting each morning regardless of whether this command also ran. This command exists for checking one person right now instead of waiting for the next scheduled run. Once the owner has reviewed and checked boxes in the resulting note, `/meeting-prep-apply <name>` compiles them into the actual talking-points list, exactly as it would for a note the daily trigger produced - this command and that scheduled task write to the same place in the same shape, so `/meeting-prep-apply` doesn't need to know or care which one produced any given note.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
