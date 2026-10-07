---
type: person
role: <Role, e.g. Product Manager, Rogue Raccoons / HR Business Partner>
facet: Agenda Log
company: Vaimo
tags: [person, pm, agenda-log, vaimo]
date: <YYYY-MM-DD, first built>
updated: <YYYY-MM-DD, last meeting logged>
ai-first: true
---

## For future Claude
Tracks discussion themes and specific tracked action items across the <weekly/biweekly/monthly> "<Name> / Đorđe" sync (first held <YYYY-MM-DD>). <Name> is <role> and a cross-functional counterpart, not a direct report. Entries go in the Meeting log newest first, as nested bullets by category (action items, decisions, notes), not prose. Đorđe adds his pre-meeting questions under "Open agenda items"; when <Name> answers (in the meeting, Slack, etc.), move the item to the meeting entry with the date answered. <PM only: Team-level strengths, weaknesses, opportunities and threats from each sync belong in [[Knowledge Base/People/<Team folder>/<Name> (PM)/<Name> - SWOT|SWOT]], not here.> See [[Knowledge Base/People/<Team folder>/<Name> (PM)/<Name> - Profile Card|Profile Card]] for relationship context.

<Optional: source doc note, e.g. built from the single running Google Doc of all notes, and the date <Name> started attending.>

<Optional, HR only: Known gap, e.g. the notes doc records almost no responses inline, so check the Gemini transcript before concluding a question went unanswered.>

<Optional: Confidential. Anything that must not be surfaced outside the vault until the person announces it.>

---

# <Name> - Agenda Log

**Agenda doc (running notes):** [Notes - <Name> / Đorđe](<Google Doc URL>)

## Recurring discussion themes

- **<Theme>:** <one line> (first raised <YYYY-MM-DD>)

## Commitments / promises tracked

<Optional, mainly HR. Owned by <Name> unless noted. Keep only items with a date or a clear owner; check status at the next sync.>

1. **<Commitment>** - <one line>. **Deadline: <date or "none given">.** *(<YYYY-MM-DD>)*

## Open agenda items

Đorđe's pre-meeting questions. Tag any sourced from someone else's notes with `(from: [[Person]])`.

- **<Question or topic>** - <one line of context>

## Meeting log

- **YYYY-MM-DD** (<context, e.g. first sync, N min>)
    - Agenda: [Notes - <Name> / Đorđe](<Google Doc URL>)
    - **Action items** as tracked commitments:
        - [<Name>] <action>
        - [Đorđe] <action> (task created YYYY-MM-DD, if tracked on the board)
    - **Decisions / confirmed answers:**
        - <Decision or answer, one line>
    - **Team notes** (feed into SWOT)
        - <PM only. Prefix each with Strength / Weakness / Opportunity / Threat.>
    - **<Name>** (career note)
        - <Optional. Role or career news and whether it is confidential.>
    - Still open, no answer:
        - <Question asked but not answered>
    - Source: [Gemini notes and transcript](<link>)

## Reference

- [[Knowledge Base/People/<Team folder>/<Name> (PM)/<Name> - Profile Card|Profile Card]]
- [[Knowledge Base/People/<Team folder>/<Name> (PM)/<Name> - SWOT|SWOT]]
- Agenda doc: <Google Doc URL>

<!--
Scope: PMs and HR contacts (Tanya Lindell, Kaisa Seppälä, Philipp Zhilin). For direct reports use "Agenda Log Template - Direct Reports".
How to use this template
1. Copy to the person's folder as "<Name> - Agenda Log.md" and fill every <placeholder>. In the tags line, swap `pm` for `hr` where it fits and add a team tag such as `rogue-raccoons` or `pink-pirates`.
2. The agenda doc link goes at the top of the file and inside every meeting entry.
3. Keep: Action items, Decisions, Still open, Source in every entry. Drop Team notes and the SWOT links for HR. Drop Commitments for PMs unless they make dated promises.
4. After each meeting, move answered items out of Open agenda items into the meeting entry with the date.
5. Create a task plus board card for each [Đorđe] action item the same pass.
6. Keep facts about the person on the Profile Card, not here.
7. Delete unused optional lines and this comment in the real file. Use plain hyphens, no em-dashes.
-->
