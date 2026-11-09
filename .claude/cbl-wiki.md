# CBL ↔ Wiki Conventions

Shared reference for how CBL commands maintain a local **Obsidian-style wiki** — a
cross-linked markdown knowledge graph that spans multiple topics and multiple CBL
challenges. Based on Karpathy's "LLM Wiki" pattern: *the wiki is the codebase, the LLM
is the programmer, Obsidian is the IDE.* Commands read this file before touching the
wiki. It is **not** a standalone command — do not run it directly.

## Why a wiki (vs an external board)

The wiki lives in the repo as plain markdown, costs ~0 extra tokens (no MCP calls, no
DSL), is version-controlled, and renders as a knowledge graph in Obsidian. CBL data is
already graph-shaped: findings cite **sources** and answer **questions**, so the graph
draws itself — Challenge → Questions → Concepts (findings) → Sources.

## The active challenge

The vault can hold **several CBL challenges at once**, each in its own folder under
`cbl-wiki/challenges/<slug>/`. A root file, **`.cbl-active`**, holds the slug of the
challenge currently being worked on (plain text, one line, e.g. `db-stress-testing`).
Every command that operates on "the" challenge:

1. Reads `.cbl-active` to get `<slug>`.
2. Operates on `cbl-wiki/challenges/<slug>/` for that challenge's own files.
3. Still reads/writes the **shared** folders (`concepts/`, `sources/`, `raw/`) at the
   vault root — those are not per-challenge.

`/project:cbl:list` shows all challenges; `/project:cbl:switch <slug>` changes
`.cbl-active`.

## Folder structure

```
cbl-wiki/
├── 00-inbox/              # quick captures, triaged later
├── daily/                 # work-day dev journal, YYYY-MM-DD.md
├── bugs/                  # one note per bug
├── concepts/              # SHARED across all challenges/topics — one idea per note
├── sources/               # SHARED — one page per Guiding Resource or cited source
├── raw/                   # SHARED — immutable input docs for /project:cbl:ingest
├── challenges/
│   └── <slug>/            # one folder per CBL challenge
│       ├── session.md     # the canonical journal (was root cbl-session.md)
│       ├── challenge.md    # Big Idea / Essential Question / Challenge (+ history in Build)
│       ├── index.md        # Map of Content for this challenge
│       ├── log.md          # dated audit trail for this challenge's wiki updates
│       ├── synthesis.md    # (Learn) or synthesis-cycle-N.md (Build)
│       └── questions/      # one page per Guiding Question
├── maps/                  # hub notes (MOCs), per-topic + one per challenge's own diagrams
└── templates/             # Obsidian note templates
```

`raw/` is **layer 1 (immutable ground truth)**: read it, never modify it; the rest of
the wiki can be recompiled from it. `concepts/` and `sources/` are **layer 2
(LLM-owned)** and shared across every challenge — the same concept can answer questions
in more than one challenge.

## Page conventions

**Filenames:** kebab-case slugs, `.md`. Link with `[[slug]]` (no path, no extension) —
Obsidian resolves by filename across the whole vault, regardless of folder. Link
liberally; a `[[slug]]` that doesn't exist yet is a valid stub marker, not an error.

**Every page** starts with YAML frontmatter:
```yaml
type: concept | question | source | challenge | index | log | daily | bug | dsa-session | hub | handbook
status: open | answered | draft     # where relevant
tags: [<topic tags>]                # e.g. database, system-design, distributed-systems, dsa, ios
challenge: <slug>                   # only on challenge-specific notes (questions, log, etc.)
```
Infer topic tags from the note's content — a note can carry several. DSA session notes
additionally carry pattern tags (`greedy`, `two-pointers`, …).

### question pages (`challenges/<slug>/questions/<question-slug>.md`)
Title = the question verbatim. Body: the category, status, and links to the
`[[concept]]` page(s) that answer it once answered, plus `[[challenge]]`. Keep stubs
short — the knowledge lives in concept pages, not here.

### concept pages (`concepts/<slug>.md`)
The actual synthesized knowledge (a finding), **shared** across challenges. Body: a
concise explanation, **Answers:** links to the `[[question]]`(s) it resolves (which may
belong to different challenges), **Sources:** links to `[[source]]` pages and/or
external URLs, and `[[concept]]` links to related ideas.

### source pages (`sources/<slug>.md`)
One per Guiding Resource (or external citation worth a node), **shared**. Body: what it
is, its URL, what it taught (links to the `[[concept]]` pages derived from it).

### synthesis pages (`challenges/<slug>/synthesis.md` or `synthesis-cycle-N.md`)
Conclusions, key insights, design principles (+ decision in Build mode). Links to the
concepts and questions it draws on.

### index.md (per challenge)
Map of Content: the challenge one-liner, a progress line (`X of Y questions answered`),
and grouped links to challenge, questions, concepts, sources, synthesis. Update it
whenever pages are added.

### log.md (per challenge)
Append one dated line per wiki update: `- YYYY-MM-DD — <what changed>`.

### daily notes (`daily/YYYY-MM-DD.md`)
Work-day dev journal — not CBL-specific. Sections: Worked on, Learned, Blockers,
Questions for my senior, Tomorrow. See `templates/daily.md`.

### bug notes (`bugs/<slug>.md`)
One per bug. Sections: Symptom, Environment, Steps to reproduce, Root cause, Fix,
Lesson, Related concepts, Date. See `templates/bug.md`.

### DSA session notes (`challenges/dsa/sessions/DayN.md`)
One note per practice session (not per problem). See `templates/dsa-session.md`.
Pattern concepts live in the shared `concepts/` folder, tagged `dsa`.

## Operations (mapped to CBL commands)

- **start** → create `cbl-wiki/challenges/<new-slug>/` with its own `session.md`,
  `challenge.md`, `index.md`, `log.md`, and set `.cbl-active` to the new slug.
- **investigate** → create a `challenges/<slug>/questions/` page per Guiding Question
  and a `sources/` page per Guiding Resource (stubs, shared). Update `index.md` +
  `log.md` for the active challenge.
- **learn (ingest)** → for each finding: create/extend a shared `concepts/` page, link
  it to the `[[question]]`(s) it answers in the active challenge (flip those to
  `status: answered`) and the `[[source]]`(s) it came from. Update `index.md` +
  `log.md`.
- **synthesize** → write `challenges/<slug>/synthesis.md` (or `synthesis-cycle-N.md`)
  linking the relevant concepts/questions, then **lint**. Update `index.md` + `log.md`.
- **ingest** (`/project:cbl:ingest`) → compile documents in the shared `raw/` into
  `sources/` summaries + extracted `concepts/`, with topic tags and backlinks to the
  active challenge's questions where relevant. Incremental, offline, immutable raw.
- **lint** (`/project:cbl:lint`) → run the health pass below over the **whole vault**
  (every challenge, not just the active one).

## Lint (run during synthesize, or on demand via `/project:cbl:lint`)

A health pass over the **whole vault**, in three tiers:

**Structural** (apply safe fixes automatically):
- **Orphans:** pages with no inbound links — connect or flag them.
- **Missing pages:** `[[slug]]` links pointing to files that don't exist — create stubs.
- **Coverage:** answered questions with no linked concept, concepts with no source, or
  any note missing a topic tag.

**Integrity** (flag + propose, never silently fix):
- **Contradictions:** concept pages whose claims conflict — flag both and note in that
  challenge's `log.md`.
- **Near-duplicates:** similar concepts — propose a merge.

**Enrichment** (suggest first; act only on approval):
- **Impute missing/weak data** via web search (only with consent, or `enrich` argument).
- **New article candidates** — recurring mentions that deserve their own concept page.
- **Follow-up questions** worth investigating — add good ones as `questions/` stubs.

Be conservative: never delete content. A wrong "fact" propagates across linked pages, so
prioritize contradiction-spotting.

## Viewing

Open `cbl-wiki/` as an Obsidian vault → Graph View shows the learning map across every
topic and challenge. Files are plain markdown, so they're also readable in any editor or
rendered on GitHub.
