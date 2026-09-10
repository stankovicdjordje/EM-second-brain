---
description: Pull a person's recent 1-on-1 history and build a live meeting-prep checklist, then compile whichever points you pick into talking points
category: thinking
triggers_en: ["meeting prep", "prep for my 1-on-1", "prep me for my meeting with", "generate talking points"]
triggers_es: ["preparar reunion", "puntos de conversacion para mi reunion"]
---

Use the obsidian-second-brain skill. Execute `/meeting-prep $ARGUMENTS`:

The argument is a person's name - handle typos and partial matches, and treat a request naming the owner's own manager (if the vault's `_CLAUDE.md` or calendar shows one, e.g. a recurring "weekly" 1-on-1 the owner attends as the non-organizer report) as a request for the owner's own upward-1:1 prep, using the owner's own vault folder instead of a direct report's.

This is the **live, chat-only** counterpart to `/meeting-prep-trigger`. Both pull the exact same data through the exact same process and follow the same `Templates/Meeting Prep Template.md` shape - the only difference is where the result goes. This command presents everything directly in this conversation and compiles the owner's picks into talking points right here; `/meeting-prep-trigger` writes the same checklist to an Obsidian file instead, for the owner to review later.

1. Read `_CLAUDE.md` first if it exists in the vault root, then resolve the person's name against `Knowledge Base/People/` the same way `/direct-report` does (fuzzy match across every team subfolder, confirm if ambiguous).
2. Look up this person's next (or today's, if same-day) weekly 1-on-1 on the owner's calendar, for the meeting time and video call link.
3. Search Google Drive for this pairing's 1-on-1 transcripts (the same naming convention `/direct-report` uses), sort by created time, take up to the last 4 - fewer is fine if that's all that exists. Read all of them in full. **These transcripts are the main driver of the whole note**, weighted by recency: the most recent one is the primary source for what to raise; the older ones (up to 3 more) are pattern/trend context only - what keeps recurring unresolved, whether something raised last time actually came up since.
4. Read that person's `<Person> - Profile Card.md` and `<Person> - Skills Matrix.md` in full, and scan `Tasks/📥 Backlog` and `Boards/Personal.md` for open items tied to them. Cross-check against these - don't raise something the vault already shows as resolved or applied - and pull the "Growth watch" / "Where they're headed" content straight from the Profile Card.
5. Following `Templates/Meeting Prep Template.md`'s shape (fall back to this repo's `references/meeting-prep-template.md` if the vault has none yet), build four prioritized lists - **Follow-ups**, **Challenges**, **Growth**, **Achievements** - highest priority first. **Format every item as `**(<Priority>[, <recurrence>/4 meetings])** <item description>` - the bold priority/recurrence tag ALWAYS comes first, never appended at the end of the line.** For Follow-ups and Challenges, always include how many of the transcripts read it recurred in (e.g. "3/4 meetings") - for Challenges specifically, cross-check severity against any existing vault framing of the same issue (the Profile Card's growth-watch section, Performance Review, or AI Competency Framework) rather than reading severity off a single transcript mention alone, since an issue the vault already documents as long-running or escalating outranks a first-time mention; Growth and Achievements carry just the priority. Every item must trace back to something actually in the material read above, or an explicitly open task - never invent one. If a bucket is genuinely empty, say so rather than omitting it.
6. Present this checklist directly here in chat, **numbered per item within each section** (not as literal markdown checkboxes - those aren't clickable in this conversation), e.g. "F1, F2 ... C1 ... G1 ... A1", and ask which of them the owner wants as talking points.
7. Once the owner replies with their picks, compile exactly those into a **single flat bullet outline** of talking points and questions - phrased as something to actually say out loud (a direct question addressed to the person, or a stated idea/suggestion), never a meta-instruction describing the act of asking; occasional nesting only where one point genuinely branches into a sub-question; ending with two fixed closing bullets, "Anything from your side?" then "Q/A". Present that compiled outline here in chat - this is the actual deliverable.
8. Nothing gets written to any vault file by this command - it is entirely a live conversation. If the owner wants any of this saved afterward, that is a separate, explicit request, not something this command does on its own.

HARD RULES:
- Never write to, or modify, any person's actual Profile Card, Skills Matrix, Performance Review, or AI Competency Framework file - all of it is read-only input here.
- Respect each person's own stated exclusion policy (health/family circumstances, other people's private details) when deciding what's worth surfacing.
- Never fabricate a talking point beyond what the material actually supports.

This is the sibling to `/meeting-prep-trigger` the same way `/direct-report` is to `/direct-report-trigger`: same underlying data and template, live-and-interactive here vs. written-to-Obsidian-for-later there. See `references/meeting-prep-template.md` for the shared template shape both commands follow.

---

This command does not create or update any vault note, so the usual AI-first write rule does not apply to its own output. Its inputs still must be read faithfully: sources preserved verbatim, recency respected, nothing invented beyond what `references/ai-first-rules.md`'s sourcing standard would require of a written note.
