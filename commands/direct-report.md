---
description: Flag profile card and skills matrix updates from a direct report's latest 1-on-1
category: thinking
triggers_en: ["direct report check", "review my 1-on-1", "check in on my direct report", "flag profile updates"]
triggers_es: ["revisar mi 1 a 1", "chequeo de reporte directo", "verificar actualizaciones de perfil"]
---

Use the obsidian-second-brain skill. Execute `/direct-report $ARGUMENTS`:

The argument is a direct report's name - handle typos and partial matches. This command turns a person's most recent 1-on-1 into a reviewable checklist of vault updates, so nothing gets written until the owner explicitly says which items to keep.

1. Read `_CLAUDE.md` first if it exists in the vault root, then search the vault for that person's folder under `Knowledge Base/People/` (fuzzy match on name - confirm before proceeding if the match is approximate or there's more than one candidate).
2. Find their most recent 1-on-1 with the owner in Google Drive - the running "Notes - <Person> / <Owner> - Weekly" doc plus the latest dated Gemini-transcribed meeting doc, whichever is more recent than that person's files' own `updated:` frontmatter date.
3. Read that person's existing `<Person> - Profile Card.md` and `<Person> - Skills Matrix.md` in full.
4. Compare the meeting content against both files and identify what's new: strengths/evidence not yet on the Profile Card, and skill/gap entries on the Skills Matrix that are new, resolved, or updated. Exclude personal, health, and family content, matching whatever exclusion policy is already stated in that person's own files - if a file has no stated exclusion policy, use the same default categories other people's files already exclude (health/family circumstances, other people's private details).
5. Separately, scan the same meeting for anything that reads as an open commitment, a follow-up, or an action item - flag each as a candidate task or reminder.
6. Present everything as a single Markdown checkbox list, one item per proposed change, grouped under three headings - `## Profile Card`, `## Skills Matrix`, `## Tasks & Reminders` - each line as `- [ ] <the specific addition or task, in one sentence>`. Do not write to any file and do not create any task or reminder yet.
7. Wait for the owner to say which boxes are checked. Apply only the checked items - write the Profile Card/Skills Matrix edits, and create the confirmed tasks/reminders - then report back what was actually changed.

Performance Review and AI Competency Framework are explicitly out of scope for this command - Performance Review only updates during an actual scheduled performance-review conversation, and AI Competency Framework updates are handled separately.

This makes the recurring "did anything from that 1-on-1 need to go in the vault?" check a repeatable command instead of a fresh manual pass every time - the owner reviews and picks, nothing gets assumed.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
