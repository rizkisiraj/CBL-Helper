# CBL ↔ Wiki Conventions

Shared reference for how CBL commands maintain a local **Obsidian-style wiki** — a
cross-linked markdown knowledge graph that grows as the learner investigates. Based on
Karpathy's "LLM Wiki" pattern: *the wiki is the codebase, the LLM is the programmer,
Obsidian is the IDE.* Commands read this file before touching the wiki. It is **not** a
standalone command — do not run it directly.

## Why a wiki (vs an external board)

The wiki lives in the repo as plain markdown, costs ~0 extra tokens (no MCP calls, no
DSL), is version-controlled, and renders as a knowledge graph in Obsidian. CBL data is
already graph-shaped: findings cite **sources** and answer **questions**, so the graph
draws itself — Challenge → Questions → Concepts (findings) → Sources.

## Three layers

1. **Canonical journal** — `cbl-session.md` stays the source of truth (the linear log).
   Always write it first. The wiki is derived from it and can be rebuilt at any time.
2. **The wiki** — `cbl-wiki/`, an LLM-owned folder of atomic, cross-linked pages.
3. **Schema** — this file. Edit it to change how the graph is built.

## Folder structure

```
cbl-wiki/
├── raw/              # immutable dropped source docs (PDFs, clipped articles, notes, images) — compiled by /project:cbl:ingest
├── index.md          # Map of Content: challenge, progress dashboard, links to all pages
├── log.md            # dated audit trail of every wiki update
├── challenge.md      # Big Idea / Essential Question / Challenge (+ version history in Build)
├── questions/        # one page per Guiding Question
├── concepts/         # one page per synthesized finding/idea (the knowledge itself)
├── sources/          # one page per Guiding Resource or cited source
└── synthesis/        # one page per synthesis (per cycle in Build mode)
```

`raw/` is **layer 1 (immutable ground truth)**: read it, never modify it; the rest of the
wiki can be recompiled from it. `concepts/` and `sources/` are **layer 2 (LLM-owned)**.

## Page conventions

**Filenames:** kebab-case slugs, `.md`. Link with `[[slug]]` (no path, no extension) —
Obsidian resolves by filename. Link liberally; a `[[slug]]` that doesn't exist yet is a
valid stub marker, not an error.

**Every page** starts with YAML frontmatter:
```markdown
---
type: question | concept | source | synthesis | challenge | index
status: open | answered        # questions/concepts only
category: Knowledge | Skills | Context | Impact | Correlations | Limitations   # questions
cycle: N                        # Build mode only
tags: [cbl]
---
```

### question pages (`questions/<slug>.md`)
Title = the question verbatim. Body: the category, status, and links to the
`[[concept]]` page(s) that answer it once answered, plus `[[challenge]]`. Keep stubs
short — the knowledge lives in concept pages, not here.

### concept pages (`concepts/<slug>.md`)
The actual synthesized knowledge (a finding). Body: a concise explanation, **Answers:**
links to the `[[question]]`(s) it resolves, **Sources:** links to `[[source]]` pages
and/or external URLs, and `[[concept]]` links to related ideas. This is where
cross-linking density comes from.

### source pages (`sources/<slug>.md`)
One per Guiding Resource (or external citation worth a node). Body: what it is, its URL,
what it taught (links to the `[[concept]]` pages derived from it).

### synthesis pages (`synthesis/<slug>.md`)
Conclusions, key insights, design principles (+ decision in Build mode). Links to the
concepts and questions it draws on. Name `synthesis-cycle-N.md` (Build) or
`synthesis.md` (Learn).

### index.md
Map of Content: the challenge one-liner, a progress line (`X of Y questions answered`),
and grouped links to challenge, questions, concepts, sources, synthesis. Update it
whenever pages are added.

### log.md
Append one dated line per wiki update: `- YYYY-MM-DD — <what changed>`.

## Operations (mapped to CBL commands)

- **start** → scaffold the vault: `index.md`, `log.md`, `challenge.md`. Record
  `**Wiki:** cbl-wiki/` in the `cbl-session.md` header.
- **investigate** → create a `questions/` page per Guiding Question and a `sources/`
  page per Guiding Resource (stubs). Update `index.md` + `log.md`.
- **learn (ingest)** → for each finding: create/extend a `concepts/` page, link it to
  the `[[question]]`(s) it answers (flip those to `status: answered`) and the
  `[[source]]`(s) it came from. Read existing pages first and strengthen links rather
  than duplicating. Update `index.md` + `log.md`.
- **synthesize** → write a `synthesis/` page linking the relevant concepts/questions,
  then **lint** (below). Update `index.md` + `log.md`.
- **ingest** (`/project:cbl:ingest`) → compile documents in `raw/` into `sources/`
  summaries + extracted `concepts/`, with backlinks. Incremental (skips already-compiled
  files), offline, immutable raw.
- **lint** (`/project:cbl:lint`) → run the health pass below on demand.

## Lint (run during synthesize, or on demand via `/project:cbl:lint`)

A health pass over the wiki, in three tiers:

**Structural** (apply safe fixes automatically):
- **Orphans:** pages with no inbound links — connect or flag them.
- **Missing pages:** `[[slug]]` links pointing to files that don't exist — create stubs.
- **Coverage:** answered questions with no linked concept, or concepts with no source.

**Integrity** (flag + propose, never silently fix):
- **Contradictions:** concept pages whose claims conflict — flag both and note in `log.md`.
- **Near-duplicates:** similar concepts — propose a merge.

**Enrichment** (suggest first; act only on approval):
- **Impute missing/weak data** via web search (only with consent, or `enrich` argument).
- **New article candidates** — recurring mentions that deserve their own concept page.
- **Follow-up questions** worth investigating — add good ones as `questions/` stubs.

Be conservative: never delete content. A wrong "fact" propagates across linked pages, so
prioritize contradiction-spotting.

## Viewing

Open `cbl-wiki/` as an Obsidian vault → Graph View shows the learning map. Files are
plain markdown, so they're also readable in any editor or rendered on GitHub.
