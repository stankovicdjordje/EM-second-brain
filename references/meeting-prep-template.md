---
type: meeting-prep
person: <Full Name>
role: <Role, e.g. Backend Engineer / Competence Lead>
company: <Company>
tags: [meeting-prep, 1-on-1, weekly, direct-report]
date: <YYYY-MM-DD, date of the meeting this note preps for>
meetings-reviewed: [<date1>, <date2>, <date3>, <date4>]
ai-first: true
---

## For future Claude
Meeting-prep checklist for <Name>'s weekly 1:1 on <date> - generated *ahead of* the meeting by `/meeting-prep`, `/meeting-prep-trigger`, or the `meeting-prep-daily-trigger` scheduled task, not a record of it (contrast with the after-the-fact review notes in `<Name>'s folder>/Transcripts/Done/`). Synthesized across the **last 4 meetings** for this person, plus [[<Name> - Profile Card]] and [[<Name> - Skills Matrix]] for standing context. Every item traces back to something actually said/recorded in that material or an explicitly open task in `Tasks/` - nothing here is invented. State plainly, here, which of the last 4 meetings were used and how each was sourced (a processed `Transcripts/Done/` note vs. a raw transcript read directly) - never pad if genuinely fewer than 4 exist.

**Nothing has been sent anywhere yet.** The owner is welcome to add their own extra checkbox items under any of the three headings below before checking the master box - `/meeting-prep-apply` picks up anything checked there too, not just what the automation itself proposed. Checking the master box is the trigger to compile whatever else is checked into a talking-points-and-questions bullet list and move this note to `Meeting Prep/Done/` - it does not edit any other vault file.

---

# <Name> - Meeting Prep - <date>

**Meeting:** <event title> · <time>
**Meetings reviewed (last 4, newest first):** <date1> (<Done note or raw transcript>) · <date2> (...) · <date3> (...) · <date4> (...)

## Review status
- [ ] I have reviewed this note and selected my talking points

## Follow-ups
Open commitments and unresolved threads carried from prior meetings, highest priority first. Each item notes how many of the last 4 meetings it's recurred in - recurrence itself is a priority signal.

- [ ] **(High, 3/4 meetings)** <item> - <what to ask or check now>
- [ ] **(Medium, 1/4 meetings)** <item> - <what to ask or check now>

## Challenges
Obstacles the person is currently facing, highest priority first - both technical/delivery-team challenges (blockers, scoping fights, tooling or process friction) and personal ones (workload strain, confidence gaps, career-pressure points) they've actually raised. Like Follow-ups, each item notes recurrence across the last 4 meetings gathered - recurrence is a severity signal here too. Cross-check severity against any existing vault framing of the same issue (this person's Profile Card growth-watch section, Performance Review, or AI Competency Framework) rather than reading severity from a single transcript mention alone - a challenge the vault already documents as long-running or escalating outranks a first-time mention.

- [ ] **(High, 2/4 meetings)** <item> - <what to ask or check now>
- [ ] **(Medium, 1/4 meetings)** <item> - <what to ask or check now>

## Growth
Career development, skill/competency progress, and goal check-ins worth raising - highest priority first.

- [ ] **(High)** <e.g. overdue Q3 goal-setting conversation, or a stalled development thread>
- [ ] **(Medium)** <e.g. an ongoing interest or in-progress skill, nothing urgent this cycle>

## Achievements
Wins and completed items worth explicitly acknowledging - highest priority first.

- [ ] **(High)** <e.g. shipped work, completed certification, resolved a long-open item>
- [ ] **(Low)** <minor but worth a nod>

*(Any bucket with nothing to report keeps its heading with a single "- Nothing to report" line, rather than being omitted, so a future run can tell "checked, empty" apart from "not checked.")*
