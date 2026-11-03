# CBL-Helper

A Challenge Based Learning (CBL) toolkit implemented as Claude Code custom slash commands.

## What is CBL?

Challenge Based Learning is a framework for learning while solving real-world challenges, originally developed by Apple (2008). It has three phases: **Engage**, **Investigate**, and **Act**, with ongoing reflection and documentation throughout.

## Modes

The toolkit supports two modes, chosen at the start of each session:

### Learn Mode
For when you want to deeply understand a topic. The challenge is clear from the start, investigation builds knowledge, and the Act phase produces a capstone project demonstrating what you learned. Flow is mostly linear.

### Build Mode
For when you want to solve a real problem or make a product. The challenge starts broad and evolves through multiple investigation cycles (problem discovery → problem validation → solution research). Each cycle ends with a synthesis that can refine the challenge or move to the next phase. The challenge is versioned (`v1`, `v2`, ...) so evolution is visible.

## Available Commands

All commands are under `/project:cbl:*`:

| Command | Phase | Purpose |
|---------|-------|---------|
| `/project:cbl:start` | Engage | Begin a new CBL journey — choose mode, define Big Idea, Essential Question, and Challenge |
| `/project:cbl:investigate` | Investigate | Generate Guiding Questions, plan activities and resources. In Build mode, calibrated to current investigation cycle focus |
| `/project:cbl:learn` | Investigate | Log a learning, finding, or insight |
| `/project:cbl:synthesize` | Investigate | Synthesize findings. In Build mode, offers fork: refine challenge & investigate deeper, or move to act |
| `/project:cbl:act` | Act | Define solution concept and implementation plan |
| `/project:cbl:progress` | Act | Log implementation progress |
| `/project:cbl:evaluate` | Act | Evaluate the solution and reflect on the full journey |
| `/project:cbl:ingest` | Any | Compile documents in `cbl-wiki/raw/` into source summaries + extracted concepts with backlinks |
| `/project:cbl:lint` | Any | Wiki health check — orphans, broken links, contradictions, missing data, new article/question candidates |
| `/project:cbl:status` | Any | View current session status and next steps |
| `/project:cbl:reflect` | Any | Quick reflection check-in |
| `/project:cbl:pivot` | Any | Adjust or change Challenge direction |

## Typical Flow

### Learn Mode
```
/project:cbl:start [topic]     --> Engage: Choose mode -> Big Idea -> Essential Question -> Challenge
/project:cbl:investigate       --> Investigate: Guiding Questions -> Activities -> Resources
/project:cbl:learn [finding]   --> (repeat as you learn things)
/project:cbl:synthesize        --> Synthesize findings into conclusions
/project:cbl:act               --> Act: Capstone project -> Implementation plan
/project:cbl:progress [update] --> (repeat as you build)
/project:cbl:evaluate          --> Evaluate solution + reflect on learning
```

### Build Mode
```
/project:cbl:start [topic]     --> Engage: Choose mode -> Big Idea -> Essential Question -> Challenge (v1)
/project:cbl:investigate       --> Cycle 1 — Problem Discovery: Guiding Questions -> Activities
/project:cbl:learn [finding]   --> (repeat as you learn things)
/project:cbl:synthesize        --> Synthesize -> Refine challenge (v2) -> Next cycle focus
/project:cbl:investigate       --> Cycle 2 — Problem Validation: New Guiding Questions
/project:cbl:learn [finding]   --> (repeat)
/project:cbl:synthesize        --> Synthesize -> Move to Solution Research
/project:cbl:investigate       --> Cycle 3 — Solution Research: New Guiding Questions
/project:cbl:learn [finding]   --> (repeat)
/project:cbl:synthesize        --> Synthesize -> Move to Act
/project:cbl:act               --> Act: Product build -> Implementation plan
/project:cbl:progress [update] --> (repeat as you build)
/project:cbl:evaluate          --> Evaluate solution + reflect on journey + challenge evolution
```

The number of cycles varies — you might skip validation if confident, or do extra cycles for complex problems.

## Session File

All session state is tracked in `cbl-session.md` at the project root. This file is created by `/project:cbl:start` and updated by all other commands.

In Build mode, the session file additionally tracks:
- **Challenge History** — version table showing how the challenge evolved
- **Investigation Cycles** — each cycle with its own questions, findings, and synthesis
- **Challenge version** — current version number in the header

## Wiki Integration (Obsidian-style knowledge graph)

CBL commands maintain a local **wiki** — a folder of cross-linked markdown pages that
grows as the learner investigates. Based on Karpathy's "LLM Wiki" pattern: the wiki is
the codebase, the LLM is the programmer, Obsidian is the IDE. CBL data is already
graph-shaped (findings cite **sources** and answer **questions**), so the graph draws
itself: Challenge → Questions → Concepts → Sources.

It's plain markdown in the repo — no external service, no MCP, ~0 extra tokens, version
controlled — and renders as a graph in Obsidian.

**Source of truth:** `cbl-session.md` stays canonical (the linear journal). The wiki in
`cbl-wiki/` is derived from it and can be rebuilt at any time.

**Structure:**
```
cbl-wiki/
├── raw/            # immutable dropped source docs — compiled by /project:cbl:ingest
├── index.md        # Map of Content + progress
├── log.md          # dated update trail
├── challenge.md    # Big Idea / Essential Question / Challenge (+ versions in Build)
├── questions/      # one page per Guiding Question
├── concepts/       # one page per synthesized finding (the knowledge)
├── sources/        # one page per Guiding Resource / cited source
└── synthesis/      # one page per synthesis (per cycle in Build mode)
```

**What each command does:**
- `start` — scaffolds the vault (`index.md`, `challenge.md`, `log.md`) and records
  `**Wiki:** cbl-wiki/` in the session header.
- `investigate` — a `questions/` page per Guiding Question and a `sources/` page per
  Guiding Resource.
- `learn` — *ingest* (narrated): the finding becomes a `concepts/` page linked to the
  `[[question]]` it answers (flipped to `status: answered`) and its `[[source]]`.
- `ingest` — *ingest* (from documents): compiles files dropped in `raw/` into `sources/`
  summaries + extracted `concepts/` with backlinks. Incremental, offline, immutable raw.
- `synthesize` — a `synthesis/` page linking the concepts/questions, then a *lint* pass.
- `lint` — health check on demand: orphans, broken links, contradictions, plus
  enrichment (web-imputed gaps, new article/question candidates) with consent.

**Global counterparts:** `/log-resource`, `/log-concept`, `/kb-ingest`, `/kb-lint` do the
same for a single long-lived vault shared across all projects (see `~/.claude/global-knowledge.md`).

**Conventions:** all wiki behavior (folder layout, frontmatter, linking, lint) lives in
`.claude/cbl-wiki.md`, which the commands read first. Edit that one file to change how
the graph is built. Open `cbl-wiki/` as an Obsidian vault to see the Graph View.
