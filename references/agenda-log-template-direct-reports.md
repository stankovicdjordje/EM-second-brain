---
type: person
role: <Role, e.g. Backend Engineer>
facet: Agenda Log
company: Vaimo
tags: [person, direct-report, agenda-log, team-name, vaimo]
date: <YYYY-MM-DD, first built>
updated: <YYYY-MM-DD, last meeting logged>
ai-first: true
---

## For future Claude
Tracks what was planned versus what actually got discussed in the <weekly/biweekly> "<Name> / Đorđe" 1-on-1 (first held <YYYY-MM-DD>). Each meeting entry sorts the agenda from the running Google Doc into **Covered / Partially covered / Not covered / Action items** by checking it against the Gemini transcript, not just the summary. Entries go newest first, as nested bullets, not prose. "Open agenda items" holds everything carried forward; when an item is answered, move it into the meeting entry with the date. <Name>'s profile and skills evidence live in [[Knowledge Base/People/<Team folder>/<Name> (<XX>)/<Name> - Profile Card|Profile Card]] and [[Knowledge Base/People/<Team folder>/<Name> (<XX>)/<Name> - Skills Matrix|Skills Matrix]]; this file only tracks meeting-to-meeting follow-through. <Exclusions carried from the Profile Card, e.g. compensation details, health or family circumstances, other people's private details.>

<Optional: Gemini transcription pitfalls for this person, e.g. how it misspells their name or Vaimo, or terms it mishears.>

---

# <Name> - Agenda Log

**Agenda doc (running notes):** [Notes - <Name> / Đorđe - <Weekly/Biweekly>](<Google Doc URL>)

## Recurring discussion themes

- **<Theme>:** <one line> (first raised <YYYY-MM-DD>)

## Open agenda items

Carried forward to the next meeting. Tag items sourced from someone else's notes with `(from: [[Person]])`.

- **<Item>** - <one line on why it is open: deferred, not asked, no answer>

## Meeting log

- **YYYY-MM-DD** (<context, e.g. first biweekly; N min>)
    - Agenda: [Notes - <Name> / Đorđe - <Weekly/Biweekly>](<Google Doc URL>)
    - **Covered**
        - <Agenda item>: <what was said, one line>
    - **Partially covered**
        - <Agenda item>: <what was said, what is missing>
    - **Not covered**
        - <Agenda item> (<why, e.g. out of time, deferred by Đorđe>)
    - **Action items** as tracked commitments:
        - [<Name>] <action>
        - [Đorđe] <action> (task created YYYY-MM-DD, if tracked on the board)
    - **Outside the agenda** (raised by <Name>)
        - <Point not on the agenda that is worth keeping>
    - Still open, no answer:
        - <Question asked but not answered>
    - Source: [Gemini notes and transcript](<link>)

## Reference

- [[Knowledge Base/People/<Team folder>/<Name> (<XX>)/<Name> - Profile Card|Profile Card]]
- [[Knowledge Base/People/<Team folder>/<Name> (<XX>)/<Name> - Skills Matrix|Skills Matrix]]
- Agenda doc: <Google Doc URL>

<!--
Scope: direct reports only. Tanya, Kaisa and other non-direct-report contacts keep their own log format (see their Agenda Logs); do not use this one for them.
How to use this template
1. Copy to the person's folder as "<Name> - Agenda Log.md" and fill every <placeholder>. In the tags line, replace `team-name` with the team tag, e.g. `rogue-raccoons` or `pink-pirates`.
2. The agenda doc link goes at the top of the file and inside every meeting entry.
3. After each meeting: sort each agenda item into Covered / Partially covered / Not covered against the full transcript. Do not trust the Gemini summary alone.
4. Move Not covered and unanswered items into Open agenda items; move answered ones into the meeting entry with the date.
5. Create a task plus board card for each [Đorđe] action item the same pass.
6. Keep facts about the person on the Profile Card and Skills Matrix, not here.
7. Delete empty headings (e.g. Outside the agenda) and this comment in the real file. Use plain hyphens, no em-dashes.
-->
