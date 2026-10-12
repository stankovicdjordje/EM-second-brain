---
description: Rebuild the Team Competency Map - a one-page mind map of direct reports' current knowledge, gaps, mentoring matches and technical direction - from the latest vault data
category: thinking
triggers_en: ["update team competency map", "refresh team skills map", "team knowledge mind map", "rebuild competency map", "team skills mind map"]
triggers_es: ["actualizar mapa de competencias del equipo", "mapa de habilidades del equipo"]
---

Use the obsidian-second-brain skill. Execute `/team-competency-map $ARGUMENTS`:

Regenerates the Team Competency Map from the current state of every direct report's vault files, fills the vault template, saves the result in the vault, and republishes it to the same private artifact URL. `$ARGUMENTS` is optional free text (e.g. "exclude Vladimir", "add the new SOK people") - apply it on top of the defaults below.

**Fixed locations (relative to vault root):**
- Template: `Templates/Team Competency Map Template.html`
- Tracking note: `Knowledge Base/Professional Development/Engineering Manager/Team Competency Map/Team Competency Map.md` - holds the artifact URL (`artifact:` frontmatter), exclusions, data schema, changelog and the previous run's JSON snapshot.
- Generated page: `Knowledge Base/Professional Development/Engineering Manager/Team Competency Map/Team Competency Map.html`

1. Read `_CLAUDE.md` if it exists in the vault root, then read the tracking note in full (previous snapshot, exclusions, schema).
2. **Find the people.** Glob `Knowledge Base/People/**/* - Skills Matrix.md`. Skip the owner's own folder (`(me)`), HR and EM peer folders (e.g. Kaisa Seppälä, Katriina Kansanaho), and anyone whose Profile Card says they no longer report to the owner (reporting line moved, handed over). Carry forward the tracking note's exclusions unless the files now say otherwise. Report anyone newly added or newly excluded vs. the snapshot.
3. **Extract per person** - read IN FULL `<Person> - Skills Matrix.md`, `<Person> - Profile Card.md`, `<Person> - AI Competency Framework.md` and every reviewed 1:1 note in `Transcripts/Done/` (`[x]` items were applied; `[ ]` items are weaker evidence). **Never open Performance Review files.** Technical competencies only - never capture health, family, pay, HR-process or other people's private details, and respect each file's own stated exclusions. Be strict and literal; the first version of this map overreached by treating self-ratings as fact, inventing gaps from "0" self-ratings, and listing skills people already had as directions. Build one object per person, schema v2 (`n,t,d,r,s,sr,g,h,ai,q,note`):
   - `s` **demonstrated** strengths (max 8): backed by 1:1 notes, Profile Card strengths, manager assessment, or concrete work described in the files. A bare self-rating never qualifies.
   - `sr` **self-rated only** strengths (max 8): top-level ("Good"/3) in the self-evaluated spreadsheet with no other evidence. A tag is never in both `s` and `sr`. The page shows these dashed and counts them separately.
   - `g` gaps (max 6): ONLY if the files name it as a gap/need for this person, or say their current/upcoming project needs it and they lack it. A self-rated 0 / "no experience" alone is NOT a gap; irrelevant technologies are never gaps. Empty is fine. Certification tags (everything in the template's `Certifications` area - GitHub Copilot, Claude, Sanity, Adobe, ISTQB, Commercetools, Shopify, Next.js, Vercel and Contentful certification - plus any new certification tag) are never gaps - a wanted or recommended certification goes in `h`.
   - `h` heading (max 4): stated goals, interests, Job Focus field, courses, planned moves. Never a tag already in `s`; a tag in `sr` only if they're actively building it. Empty is fine.
   - `ai`: only a level written in the AI Competency Framework file or an explicit EM read in the files; otherwise null.
   - `q`: `rich` (1:1/manager evidence AND skills data) | `mixed` (strong in one area, thin elsewhere) | `self` (spreadsheet only) | `sparse` | `none`.
   - `note`: one neutral sentence of stated facts. No inferred verbs ("leads", "owns", "expert") unless the files use them; say when the stack is self-rated.
   - `t` = team folder name without the emoji.
   - Certification nodes: put a person in `s` for a certification tag only when the certification is confirmed by the owner; self-reported certifications go in `sr`.
   - `AI automation`: AI work that runs without a person prompting each step (hooks, CI, scheduled/triggered agents, parallel sub-agents, connected pipelines). Interactive chat use is `AI-assisted dev`, not automation. Full and partial (semi-automated, person-triggered) automation both go in `s`, with no label distinguishing them.
   - `u` last-updated date (YYYY-MM-DD), shown next to the person's name. Compare each person's new object with the snapshot: if anything in it changed, set `u` to today; if nothing changed, keep the snapshot's `u`. Any single-person edit made on the owner's request also sets that person's `u` to today.
   - `c` (optional) certifications: full names as listed in the Profile Card `## Certifications` section or the Skills Matrix certifications line. Shown in the person panel only; a certification never makes a skill demonstrated.
   - Use the exact tag spellings already in the template's `DOMAINS` lists. With 30+ people, extract in parallel subagents (one per team folder), and have each return per-tag evidence pointers (`file (date): short quote`) plus an overall `high|medium|low` confidence and concerns per person. Merge, normalise tags in one pass, then **spot-check yourself** at least one entry per subagent against its source files before generating. Evidence stays out of the page data; summarise low-confidence entries in the chat reply.
4. **New skill tags.** If a tag doesn't exist in any `DOMAINS` list, first try to map it to an existing tag (synonym, version, vendor name). Only if it's genuinely new, add it to the single best-fitting `DOMAINS` list in the template (and to a `THEMES` tag list if it can be a direction), and mention the template change in the summary. An unmapped tag is silently dropped from the map.
5. **Generate.** Copy the template and replace exactly: the `/*@@PEOPLE_DATA@@*/[]` marker with the JSON array (order people by team, largest team first, then by name), `{{AS_OF}}` with today's date (e.g. `5 Oct 2026`), `{{EXCLUDED_NOTE}}` with one plain sentence naming who is left out and why plus anyone with no data. Extract the inline `<script>` and run `node --check` on it before saving. Write the result to the generated-page path (overwrite).
6. **Publish.** Republish the generated page to the URL in the tracking note's `artifact:` frontmatter (Artifact tool: `action: "read"` on that URL first, then publish with `url` set - never create a new artifact unless that URL is gone, in which case publish a new one and update `artifact:`). Leave its sharing exactly as it is.
7. **Diff and update the tracking note.** Compare the new data with the previous snapshot and add a dated `## Changelog` entry: people added/removed, gaps closed (a tag moved from `g` to `s`), new gaps, direction changes, self-rated strengths now confirmed (`sr` -> `s`), data-quality upgrades (e.g. `sparse` -> `rich`). Then refresh `updated:`, `people-count:`, the team counts line, `## Excluded`, `## Data quality`, `## Headline read` (recompute the counts from the data - don't estimate), and replace `## Data snapshot` with the new JSON. Regenerate `Team Competency Map - Evidence.md` in the same folder: per person, data quality, confidence, every `s`/`g`/`h` tag with its evidence pointer, the self-rated-only list, remaining doubts, and changes since the last run.
8. Reply in chat with the artifact link, the changelog entry, and anything that needs the owner's eye (new exclusions, template vocabulary changes, people still without data). Don't paste the whole data set.

HARD RULES:
- Never edit any person's Profile Card, Skills Matrix, AI Competency Framework or Performance Review file - this command only reads them.
- Never read Performance Review files.
- Never make the artifact public or share it - sharing stays the owner's call from the page's Share menu.

---

**AI-first rule:** Every note created or updated by this command MUST follow `references/ai-first-rules.md` - `## For future Claude` preamble, rich frontmatter (`type`, `date`, `tags`, `ai-first: true`, plus type-specific fields), recency markers per external claim, mandatory `[[wikilinks]]` for every person/project/concept referenced, sources preserved verbatim with URLs inline, and confidence levels where applicable. If that path does not resolve from your working directory, search upward for it; if you still cannot read it, say so before writing rather than producing a note that silently skips the rule. The vault is for future-Claude retrieval - not human reading.
