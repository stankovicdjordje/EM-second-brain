---
description: Apply a checked-off /direct-report review note to a person's vault files
category: thinking
triggers_en: ["apply review", "apply direct report review", "apply the checklist", "pick up the review note"]
triggers_es: ["aplicar la revisión", "aplicar el checklist"]
---

Use the obsidian-second-brain skill. Execute `/direct-report-apply-review $ARGUMENTS`:

The argument is a direct report's name, optionally followed by a date (e.g. `Dmytro Melnyk 2026-09-07`) - handle typos and partial matches. This command is the other half of `/direct-report` and the `direct-report-transcript-poll` scheduled task: those produce a review note under `Logs/Direct Report Reviews/To Do/` with a single master "reviewed" checkbox plus itemized checkboxes; this command reads that note back, and if - and only if - the master checkbox is checked, applies exactly the itemized boxes that are also checked, moves the note to `Logs/Direct Report Reviews/Done/`, and confirms via Slack. **Checking the master box is the confirmation gate - do not ask for a separate approval before applying.**

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match, confirm if ambiguous).
2. Find the target review note in `Logs/Direct Report Reviews/To Do/` ONLY (never `Done/`) for that person: if a date was given, use `<Person Full Name> - <date>.md` exactly; otherwise take the most recent dated note for that person still sitting in `To Do/`. If none exists there, say so and stop - don't fabricate one, and don't look in `Done/`.
3. Read the note in full. Check its `## Review status` section for the master checkbox:
   - If it is still `- [ ] I have reviewed this note and it's ready to apply` (unchecked), **stop here and do nothing** - leave the file exactly as-is in `To Do/`. If this was invoked live in chat, tell the owner it hasn't been marked reviewed yet; if unattended, just skip silently (no Slack message).
   - If it is checked (`- [x]`), continue to step 4.
4. Parse every checkbox line under the note's `## Template Conformance`, `## Profile Card`, `## Skills Matrix`, and `## Tasks & Reminders` headings. Only lines checked `- [x]` are in scope - leave every `- [ ]` line alone, and don't infer intent beyond what's literally checked. (It's valid for the master box to be checked while zero itemized boxes are checked - that just means "reviewed, nothing to apply.")
5. Apply each checked itemized box to the right place:
   - `## Profile Card` and any `## Template Conformance` items describing Profile Card structure/content → edit `<Person> - Profile Card.md` directly, following the same conventions already established in that file (theming, brevity, dated evidence, the "For future Claude" update-note pattern) and matching `Templates/Profile Card Template.md`'s shape.
   - `## Skills Matrix` and any `## Template Conformance` items describing Skills Matrix structure/content → edit `<Person> - Skills Matrix.md` the same way, matching `Templates/Skills Matrix Template.md`.
   - `## Tasks & Reminders` → this is where the checked box actually becomes a real task for the first time: create a task note under `Tasks/📥 Backlog/` (or the appropriate status subfolder) following this vault's existing task frontmatter/format, and add the matching card to `Boards/Personal.md`, the same way tasks have been created manually elsewhere in this vault.
   - Never touch Performance Review or AI Competency Framework from this command - same standing exclusion as `/direct-report` itself.
6. Update each edited file's `updated:` frontmatter date to today.
7. Append a short `## Applied (<today's date>)` section at the bottom of the review note listing exactly what was applied and where (or "reviewed, nothing was checked to apply" if only the master box was checked).
8. Move the note from `Logs/Direct Report Reviews/To Do/<filename>` to `Logs/Direct Report Reviews/Done/<filename>` (same filename, just relocated - this is the actual completion signal, not a frontmatter flag).
9. Notify the owner: find their own Slack user (search Slack users for "Đorđe Stanković" / djordje.stankovic@vaimo.com) and send a direct message confirming what happened, e.g. "✅ <Person>'s 1-on-1 review has been applied and moved to Done. Changes: <one-line summary>." Always send this Slack confirmation regardless of whether this command was triggered live in chat or by the hourly `direct-report-apply-review-poll` scheduled task - if it was live in chat, also report the same summary there.

This closes the loop the scheduled poller opens: a new transcript triggers a checklist note in `To Do/` and a Slack ping, the owner reviews at their own pace (including directly in Obsidian, since checkbox clicks there edit the underlying markdown in place) and checks the master box when done, and either this command (run on demand) or the `direct-report-apply-review-poll` scheduled task (checking `To Do/` hourly) turns that into real vault changes, moves the note to `Done/`, and confirms back over Slack - no live chat exchange required to re-litigate what was already decided.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
