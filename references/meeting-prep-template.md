---
type: meeting-prep
person: <Full Name>
role: <Role, e.g. Backend Engineer / Competence Lead>
company: <Company>
tags: [meeting-prep, 1-on-1, weekly, direct-report]
date: <YYYY-MM-DD, date of the meeting this note preps for>
meetings-reviewed: [<date1>, <date2>, <date3>]
ai-first: true
---

## For future Claude
Meeting-prep checklist for <Name>'s weekly 1:1 on <date> - generated *ahead of* the meeting by `/meeting-prep`, `/meeting-prep-trigger`, or the `meeting-prep-daily-trigger` scheduled task, not a record of it (contrast with the after-the-fact review notes in `<Name>'s folder>/Transcripts/Done/`). Synthesized **log-first**: from the person's [[<Name> - Agenda Log]] (its themes, open items and 3 most recent meeting entries), plus a raw transcript read directly only for any meeting newer than the log's last entry - or the last 4 transcripts under `--deep`, or when no usable log exists - plus [[<Name> - Profile Card]] and [[<Name> - Skills Matrix]] for standing context. Every item traces back to something actually said/recorded in that material or an explicitly open task in `Tasks/` - nothing here is invented. State plainly, here, how fresh the sourcing was: the log's last-entry date, which of its entries were used, and which transcripts (if any) were read and why - never pad if fewer meetings exist.

**Nothing has been sent anywhere yet.** The owner is welcome to add their own extra checkbox items under any heading below before checking the master box - `/meeting-prep-apply` picks up anything checked there too, not just what the automation itself proposed. Checking the master box is the trigger to compile whatever else is checked into a talking-points-and-questions bullet list and move this note to `Meeting Prep/Done/` - it does not edit any other vault file.

---

# <Name> - Meeting Prep - <date>

**Meeting:** <event title> · <time>
**Meetings reviewed (newest first):** <date1> (<log entry, transcript, or both>) · <date2> (...) · <date3> (...)

## Review status
- [ ] I have reviewed this note and selected my talking points

## Suggested agenda
The 5-7 items most worth raising in this meeting, in the order to raise them, pulled from across all the buckets below by priority, recurrence, time pressure and risk. **Nothing here is pre-checked** - the owner ticks what they actually want. Each line is tagged with the bucket it came from so `/meeting-prep-apply` can file it under the right category. **Every item in the whole note appears exactly once:** an item listed here is NOT repeated in its bucket below, and likewise nothing is listed in two buckets (an item sourced from the Agenda Log lives in its natural bucket, tagged `(source: log)`, not again under "Agenda log open items").

- [ ] [Follow-ups] **(High, 2/3 meetings)** <item> - <what to ask or check now>
- [ ] [Challenges] **(High, 2/3 meetings)** <item> - <what to ask or check now>
- [ ] [Growth] **(High)** <item> - <what to ask or check now>

## Follow-ups
*(Only items NOT already in Suggested agenda - never repeat one.)* Open commitments and unresolved threads carried from prior meetings, highest priority first. Each item notes how many of the meetings reviewed it's recurred in (e.g. 2/3) - recurrence itself is a priority signal.

- [ ] **(High, 2/3 meetings)** <item> - <what to ask or check now>
- [ ] **(Medium, 1/3 meetings)** <item> - <what to ask or check now>

## Challenges
Obstacles the person is currently facing, highest priority first - both technical/delivery-team challenges (blockers, scoping fights, tooling or process friction) and personal ones (workload strain, confidence gaps, career-pressure points) they've actually raised. Like Follow-ups, each item notes recurrence across the meetings reviewed - recurrence is a severity signal here too. Cross-check severity against any existing vault framing of the same issue (this person's Profile Card growth-watch section, Performance Review, or AI Competency Framework) rather than reading severity from a single transcript mention alone - a challenge the vault already documents as long-running or escalating outranks a first-time mention.

- [ ] **(High, 2/3 meetings)** <item> - <what to ask or check now>
- [ ] **(Medium, 1/3 meetings)** <item> - <what to ask or check now>

## Growth
Career development, skill/competency progress, and goal check-ins worth raising - highest priority first.

- [ ] **(High)** <e.g. overdue Q3 goal-setting conversation, or a stalled development thread>
- [ ] **(Medium)** <e.g. an ongoing interest or in-progress skill, nothing urgent this cycle>

## Achievements
Wins and completed items worth explicitly acknowledging - highest priority first.

- [ ] **(High)** <e.g. shipped work, completed certification, resolved a long-open item>
- [ ] **(Low)** <minor but worth a nod>

## Company related
Recent company-wide updates worth mentioning in this 1:1, pulled from Slack (public channels only - #vaimo-company-announcements, #europe-engineering, #europe-client-delivery, and any other public channel), no older than 2 weeks from the meeting date. Highest priority first, prioritizing anything that touches this person's own team/client/role over generic company news. Each item is a short brief plus the concrete data/detail plus a direct link back to the Slack message - never a private-channel source, never older than the 2-week window.

- [ ] **(High)** <brief> - <data/detail> - [View in Slack](<permalink>)
- [ ] **(Low)** <brief> - <data/detail> - [View in Slack](<permalink>)

## Agenda log open items
*(Include this section ONLY if the person has an Agenda Log note in their folder - omit it entirely otherwise.)* Only log items that do NOT already appear in Suggested agenda or another bucket (no duplicates - an item that is a Follow-up or Challenge lives there, tagged `(source: log)`); typically the leftover "planned but not discussed" items. Items still open in [[<Name> - Agenda Log]] (last entry <date>): the "Open agenda items" list, plus "Not covered"/"Partially covered" items and unfinished action items from the latest meeting entry, reconciled against the newest transcript. Highest priority first. Check the ones to add to the meeting; the Agenda Log itself is never edited by this automation.

- [ ] **(High, carried 2 meetings)** <item as worded in the log> - <what to ask or confirm now> (source: Open agenda items)
- [ ] **(Medium, carried 1 meeting)** <item> - <what to ask or confirm now> (source: <date> Not covered)

*(Any bucket with nothing to report keeps its heading with a single "- Nothing to report" line, rather than being omitted, so a future run can tell "checked, empty" apart from "not checked.")*
