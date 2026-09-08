---
description: Compile a checked-off meeting-prep note into a talking-points-and-questions list
category: thinking
triggers_en: ["apply meeting prep", "compile my talking points", "give me my talking points", "meeting prep output"]
triggers_es: ["aplicar preparación de reunión", "compilar mis puntos de conversación"]
---

Use the obsidian-second-brain skill. Execute `/meeting-prep-apply $ARGUMENTS`:

The argument is a person's name, optionally followed by a date (e.g. `<Person> 2026-09-08`) - handle typos and partial matches, and treat "Jani" as a request for the owner's own upward-1:1 prep note (see step 1). This is the other half of the `meeting-prep-daily-trigger` scheduled task: that task writes a checklist note under a person's own `Meeting Prep/To Review/` subfolder with a single master "reviewed" checkbox plus itemized Follow-ups/Growth/Achievements checkboxes; this command reads that note back, and if - and only if - the master checkbox is checked, compiles exactly the itemized boxes that are also checked into a **bullet list of talking points and questions**, presents that list directly in chat (the actual deliverable - the owner needs this before walking into the meeting), moves the note to that same folder's `Done/`, and confirms via Slack + a desktop push notification.

**Checking the master box is the confirmation gate - do not ask for a separate approval before compiling.**

1. Read `_CLAUDE.md` first if it exists in the vault root.
2. Resolve the person:
   - If the argument names Jani (or clearly means the owner's own manager 1:1), the target folder is the owner's own folder, `Knowledge Base/People/Đorđe Stanković (me) (BE)/` - this is the owner's upward-reporting meeting prep, not a direct report's.
   - Otherwise, resolve the name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match across every team subfolder, confirm if ambiguous).
   Their `Meeting Prep/` subfolder sits alongside their facet files (`<Person> - Profile Card.md`, etc.) and, for direct reports, their `Transcripts/` subfolder.
3. Find the target note in that folder's `Meeting Prep/To Review/` ONLY (never `Meeting Prep/Done/`): if a date was given, use `<date> - <Person Full Name>.md` exactly (for the Jani case, `<Person Full Name>` is "Đorđe Stanković"); otherwise take the most recent dated note still sitting in `To Review/`. If none exists there, say so and stop - don't fabricate one, and don't look in `Done/`.
4. Read the note in full. Check its `## Review status` section for the master checkbox:
   - If it is still `- [ ] I have reviewed this note and selected my talking points` (unchecked), **stop here and do nothing** - leave the file exactly as-is, tell the owner it hasn't been marked reviewed yet.
   - If it is checked (`- [x]`), continue to step 5.
5. Parse every checkbox line under the note's `## Follow-ups`, `## Growth`, and `## Achievements` headings. Only lines checked `- [x]` are in scope - leave every `- [ ]` line alone, and don't infer intent beyond what's literally checked. (It's valid for the master box to be checked while zero itemized boxes are checked - that just means "reviewed, nothing to raise this time.")
6. Compile the checked items into a **bullet list of talking points and questions** - this is the actual output, not a restatement of the checklist:
   - Reword each checked item into how the owner would actually raise it in conversation - a talking point (a statement to make) or a question (something to ask), whichever fits the item. Keep the priority ordering from the note (highest priority first, Follow-ups tend to open the substantive part of the conversation, Growth and Achievements can interleave by priority rather than being rigidly grouped, since that's closer to how these 1:1s actually flow per the vault's own transcripts).
   - Don't just copy the checklist line verbatim - it was written as a scannable proposal, not as something to say out loud. A Follow-up like "(High, 3/4 meetings) Confirm Nemo's answer on the 80% -> 100% allocation timing" becomes something like "Ask whether Nemo has confirmed the 80% -> 100% allocation timing yet, and whether it can be announced to the team."
   - Never add a talking point that wasn't checked, and never invent detail beyond what the checked item and the note's own sourced material support.
7. Present the compiled bullet list directly in chat - this is the primary deliverable, needed before the meeting starts. Lead with a one-line header naming who the meeting is with and when.
8. Append the compiled list to the note itself, under a new `## Talking Points` section at the bottom (so it's preserved alongside the checklist it came from), then append a short `## Compiled (<today's date>)` note below that stating which itemized boxes were checked vs left unchecked (or "reviewed, nothing checked - no talking points compiled" if only the master box was checked).
9. Move the note from `<folder>/Meeting Prep/To Review/<filename>` to `<folder>/Meeting Prep/Done/<filename>` (same filename, just relocated).
10. Notify the owner in two ways, in addition to the in-chat bullet list from step 7 (always send both, regardless of how this command was invoked):
    a. **Slack**, posted to the vault's configured direct-report/meeting-prep notification channel (the same channel `meeting-prep-daily-trigger` uses - a private channel with only the owner in it. Use that channel directly, do NOT search for or DM the owner's user profile). Keep this brief - the full list already went to chat:
       ```
       🗓️ Talking points compiled for <Person>'s <date> 1:1 and moved to Done - <N> item(s).
       ```
    b. **Desktop push notification** (the `PushNotification` tool, `status: "proactive"`) - one line, under 200 characters, e.g. "🗓️ Talking points ready for <Person> - N item(s)."

HARD RULES:
- Never touch any person's Profile Card, Skills Matrix, Performance Review, or AI Competency Framework file from this command - it only ever reads a `Meeting Prep/To Review/` note, writes the compiled list back into that same note, and moves it to `Meeting Prep/Done/`.
- Never compile an itemized box that isn't checked `- [x]`.
- Never fabricate a talking point beyond what a checked item and its sourced material actually support.

This closes the loop the scheduled task opens: `meeting-prep-daily-trigger` writes a prioritized checklist and a Slack ping every morning, the owner reviews at their own pace (including directly in Obsidian, since checkbox clicks there edit the underlying markdown in place), checks the boxes they actually want to raise plus the master "reviewed" box, and this command turns that into the actual talking-points-and-questions list they walk into the meeting with - no live back-and-forth required to re-litigate what was already decided by checking boxes. See `references/meeting-prep-daily-trigger.md` for the scheduled task's own setup and prompt.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
