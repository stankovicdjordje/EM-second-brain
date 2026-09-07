---
description: Apply a checked-off /direct-report review note to a person's vault files
category: thinking
triggers_en: ["apply review", "apply direct report review", "apply the checklist", "pick up the review note"]
triggers_es: ["aplicar la revisión", "aplicar el checklist"]
---

Use the obsidian-second-brain skill. Execute `/direct-report-apply-review $ARGUMENTS`:

The argument is a direct report's name, optionally followed by a date (e.g. `<Person> 2026-09-07`) - handle typos and partial matches. This command is the other half of `/direct-report` and the `direct-report-transcript-poll` scheduled task: those produce a review note under that person's own `Transcripts/To Review/` subfolder with a single master "reviewed" checkbox plus itemized checkboxes; this command reads that note back, and if - and only if - the master checkbox is checked, applies exactly the itemized boxes that are also checked, moves the note to that same person's `Transcripts/Done/`, and confirms via Slack + a desktop push notification. **Checking the master box is the confirmation gate - do not ask for a separate approval before applying.**

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match, confirm if ambiguous). Their `Transcripts/` subfolder sits alongside their `<Person> - Profile Card.md` etc.
2. Find the target review note in that person's `Transcripts/To Review/` ONLY (never their `Transcripts/Done/`): if a date was given, use `<date> - <Person Full Name>.md` exactly; otherwise take the most recent dated note still sitting in `To Review/`. If none exists there, say so and stop - don't fabricate one, and don't look in `Done/`.
3. Read the note in full. Check its `## Review status` section for the master checkbox:
   - If it is still `- [ ] I have reviewed this note and it's ready to apply` (unchecked), **stop here and do nothing** - leave the file exactly as-is in `To Review/`. If this was invoked live in chat, tell the owner it hasn't been marked reviewed yet; if unattended, just skip silently (no Slack message, no push).
   - If it is checked (`- [x]`), continue to step 4.
4. Parse every checkbox line under the note's `## Template Conformance`, `## Profile Card`, `## Skills Matrix`, and `## Tasks & Reminders` headings. Only lines checked `- [x]` are in scope - leave every `- [ ]` line alone, and don't infer intent beyond what's literally checked. (It's valid for the master box to be checked while zero itemized boxes are checked - that just means "reviewed, nothing to apply.")
5. Apply each checked itemized box to the right place:
   - `## Profile Card` and any `## Template Conformance` items describing Profile Card structure/content → edit `<Person> - Profile Card.md` directly, following the same conventions already established in that file (theming, brevity, dated evidence, the "For future Claude" update-note pattern) and matching `Templates/Profile Card Template.md`'s shape.
   - `## Skills Matrix` and any `## Template Conformance` items describing Skills Matrix structure/content → edit `<Person> - Skills Matrix.md` the same way, matching `Templates/Skills Matrix Template.md`.
   - `## Tasks & Reminders` → this is where the checked box actually becomes a real task for the first time: create a task note under `Tasks/📥 Backlog/` (or the appropriate status subfolder) following this vault's existing task frontmatter/format, and add the matching card to `Boards/Personal.md`, the same way tasks have been created manually elsewhere in this vault. Before creating a new task, check whether it would just be adding detail to an already-open task on the same subject (e.g. a follow-up narrowing an existing open question) - if so, update that existing task instead of creating a duplicate, and say so when reporting back.
   - Never touch Performance Review or AI Competency Framework from this command - same standing exclusion as `/direct-report` itself.
6. Update each edited file's `updated:` frontmatter date to today.
7. Append a short `## Applied (<today's date>)` section at the bottom of the review note listing exactly what was applied and where (or "reviewed, nothing was checked to apply" if only the master box was checked).
8. Move the note from `<Person's folder>/Transcripts/To Review/<filename>` to `<Person's folder>/Transcripts/Done/<filename>` (same filename, just relocated - this is the actual completion signal, not a frontmatter flag).
9. Notify the owner in two ways, always, regardless of whether this command was triggered live in chat or by the hourly `direct-report-apply-review-poll` scheduled task:
   a. **Slack**, posted to the vault's configured direct-report notification channel (the same channel `direct-report-transcript-poll` and `direct-report-apply-review-poll` use - a private channel with only the owner in it. Use that channel directly, do NOT search for or DM the owner's user profile). Format as a bulleted breakdown, not a single line - the point is to make it obvious at a glance what changed vs. what didn't:
      ```
      ✅ <Person>'s 1-on-1 review has been applied and moved to Done.

      Applied:
      • <one bullet per checked item that was actually applied, plainly stated - or "Nothing checked - reviewed only" if the master box was the only thing checked>

      Left unchanged (not checked):
      • <one bullet per itemized box that was left unchecked - omit this whole section if every itemized box was checked>
      ```
   b. **Desktop push notification** (the `PushNotification` tool, `status: "proactive"`) - one line, under 200 characters, e.g. "✅ <Person>'s review applied & moved to Done - N item(s) changed." This is a genuine separate alert channel (not routed through Slack), since Slack cannot notify the owner about messages posted under their own identity.
   If this command was invoked live in chat, also report the same bulleted summary there - don't skip the in-chat report just because Slack/push were sent.

This closes the loop the scheduled poller opens: a new transcript triggers a checklist note in that person's `Transcripts/To Review/` and a notification, the owner reviews at their own pace (including directly in Obsidian, since checkbox clicks there edit the underlying markdown in place) and checks the master box when done, and either this command (run on demand) or the `direct-report-apply-review-poll` scheduled task (checking every person's `Transcripts/To Review/` hourly via a recursive scan) turns that into real vault changes, moves the note to `Transcripts/Done/`, and confirms back - no live chat exchange required to re-litigate what was already decided. See `references/direct-report-apply-review-poll.md` for the full task prompt and how to set up the trigger.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
