---
description: Apply a checked-off /direct-report review note to a person's vault files
category: thinking
triggers_en: ["apply review", "apply direct report review", "apply the checklist", "pick up the review note"]
triggers_es: ["aplicar la revisión", "aplicar el checklist"]
---

Use the obsidian-second-brain skill. Execute `/direct-report-apply-review $ARGUMENTS`:

The argument is a direct report's name, optionally followed by a date (e.g. `Dmytro Melnyk 2026-09-07`) - handle typos and partial matches. This command is the other half of `/direct-report` and the `direct-report-transcript-poll` scheduled task: those produce an unchecked checklist note for the owner to review (in Obsidian or anywhere else); this command reads back whichever boxes the owner has since checked and applies exactly those, nothing more. **Checking a box in the note IS the confirmation - do not ask for a second round of approval before applying.**

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match, confirm if ambiguous).
2. Find the target review note under `Logs/Direct Report Reviews/` for that person: if a date was given, use `<Person Full Name> - <date>.md` exactly; otherwise take the most recent dated note for that person that isn't already marked processed (see step 6). If none exists, say so and stop - don't fabricate one.
3. Read the note in full and parse every checkbox line under its `## Template Conformance`, `## Profile Card`, `## Skills Matrix`, and `## Tasks & Reminders` headings. Only lines checked `- [x]` are in scope - leave every `- [ ]` line alone, and don't infer intent beyond what's literally checked.
4. Apply each checked item to the right place:
   - `## Profile Card` and any `## Template Conformance` items describing Profile Card structure/content → edit `<Person> - Profile Card.md` directly, following the same conventions already established in that file (theming, brevity, dated evidence, the "For future Claude" update-note pattern) and matching `Templates/Profile Card Template.md`'s shape.
   - `## Skills Matrix` and any `## Template Conformance` items describing Skills Matrix structure/content → edit `<Person> - Skills Matrix.md` the same way, matching `Templates/Skills Matrix Template.md`.
   - `## Tasks & Reminders` → this is where the checked box actually becomes a real task for the first time: create a task note under `Tasks/📥 Backlog/` (or the appropriate status subfolder) following this vault's existing task frontmatter/format, and add the matching card to `Boards/Personal.md`, the same way tasks have been created manually elsewhere in this vault.
   - Never touch Performance Review or AI Competency Framework from this command - same standing exclusion as `/direct-report` itself.
5. Update each edited file's `updated:` frontmatter date to today.
6. Mark the review note itself as processed: add a short `## Applied (<today's date>)` section at the bottom listing exactly what was applied and where (or "nothing was checked" if every box was left unchecked), and add `processed: true` to its frontmatter so a later run of this command (or a human re-reading `Logs/Direct Report Reviews/`) doesn't re-apply it. Do not delete or move the note - it stays as a dated record.
7. Report back to the owner in chat exactly what was changed or created, file by file.

This closes the loop the scheduled poller opens: a new transcript triggers a checklist note and a Slack ping, the owner checks boxes wherever is convenient (including directly in Obsidian, since checkbox clicks there edit the underlying markdown in place), and this command is what actually turns those checks into vault changes and real tasks - without needing a live chat exchange to re-litigate what was already decided.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
