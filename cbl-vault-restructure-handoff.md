# Handoff: Restructure my CBL-Helper vault for multiple topics

You're working in my repo `CBL-Helper`. It's a Challenge Based Learning toolkit: custom slash commands in `.claude/commands/cbl/*.md` that maintain an Obsidian vault in `cbl-wiki/`. Read `CLAUDE.md`, `.claude/cbl-wiki.md`, `cbl-session.md`, and every file in `.claude/commands/cbl/` before changing anything.

## Current state (verify this yourself)
- The vault is organized **by note type**: `concepts/`, `questions/`, `sources/`, `raw/`, `maps/`, plus root files `index.md`, `challenge.md`, `log.md`, `dashboard.md`, and `presentation.md`.
- It holds **one** CBL challenge, "Database Stress-Testing & Monitoring System". Its session file is `cbl-session.md` at the repo root.
- The commands hardcode paths like `cbl-wiki/questions/`, `cbl-wiki/challenge.md`, `cbl-wiki/log.md`, `cbl-wiki/index.md`, and `cbl-wiki/synthesis/synthesis.md`.
- Every note's frontmatter has `tags: [cbl]`, so tags don't tell topics apart.
- The graph color groups (`cbl-wiki/.obsidian/graph.json`) are based on `path:`.

## Goal
One vault that holds several topics: databases/system design (existing), distributed systems, DSA, and an iOS handbook. It should also hold a daily dev journal and bug logs. Concepts are **shared across topics**. Each CBL challenge gets **its own folder**.

## Target structure
```
cbl-wiki/
├── 00-inbox/              # quick captures (mostly from my tablet)
├── daily/                 # work-day dev journal, YYYY-MM-DD.md (not DSA)
├── bugs/                  # one note per bug
├── concepts/              # SHARED across all topics, one idea per note
├── sources/               # SHARED
├── raw/                   # SHARED, input docs for /cbl:ingest
├── challenges/
│   ├── db-stress-testing/ # the existing challenge moves here
│   │   ├── session.md     # was ../../cbl-session.md
│   │   ├── challenge.md, index.md, log.md, dashboard.md, presentation.md
│   │   ├── synthesis.md   # created when needed
│   │   └── questions/
│   ├── dsa/               # README + questions/ + sessions/ (see task 5)
│   │   └── sessions/      # my day-by-day practice logs: Day1.md, Day2.md, Day3.md…
│   └── ios-handbook/      # scaffold only
├── maps/                  # hub notes (MOCs) + learning-path.md + knowledge-map.canvas
└── templates/
```

## Tasks

### 1. Restructure (no content loss)
- Work on a new branch, `restructure-multi-topic`.
- Move files with `git mv` so history is kept.
- Keep note **filenames unchanged** so `[[wikilinks]]` still resolve (Obsidian resolves links by basename). Only touch links that would break or become ambiguous.
- Don't rewrite the body content of existing notes, apart from link fixes and the frontmatter changes in task 2.
- Add `.gitkeep` to empty folders.

### 2. Frontmatter and tags
Use this schema on all notes:
```yaml
type: concept | question | source | challenge | index | log | daily | bug | dsa-session | hub | handbook
status: open | answered | draft     # where relevant
tags: [<topic tags>]                # e.g. database, system-design, distributed-systems, dsa, ios
challenge: db-stress-testing        # only on challenge-specific notes (questions, log, etc.)
```
- Replace `cbl` with real topic tags. Infer them from each note's content; DSA session notes also get pattern tags (`greedy`, `two-pointers`…). Most current notes are `database` and/or `system-design`, and a few are `distributed-systems` (e.g. Cassandra, CAP, scaling, tail latency). Notes can have several tags.
- Show me the proposed tag for each note in one table **before** applying it.

### 3. Make the commands support multiple challenges
- Add a root file `.cbl-active` containing the active challenge slug (e.g. `db-stress-testing`). Every command reads it to find `cbl-wiki/challenges/<slug>/`.
- Update all commands in `.claude/commands/cbl/` to use the new paths:
  - `start`: creates a new `challenges/<slug>/` folder with its own `session.md`, and sets `.cbl-active`.
  - `ingest`: still writes concepts to the shared `concepts/` and summaries to `sources/`, adds topic tags, and links to the active challenge's questions where relevant.
  - `lint`: checks the **whole vault** (orphans, broken links, missing frontmatter, notes with no topic tag).
  - `status`, `learn`, `investigate`, `synthesize`, `act`, `progress`, `evaluate`, `reflect`, `pivot`: operate on the active challenge.
- Add two new commands:
  - `/project:cbl:list`: list challenges with mode, phase, and progress.
  - `/project:cbl:switch <slug>`: change `.cbl-active`.
- Update `CLAUDE.md` and `.claude/cbl-wiki.md` to describe the new structure, the frontmatter schema, and the new commands.

### 4. Templates (Obsidian core Templates syntax: `{{title}}`, `{{date}}`)
The vault has **four separate note families**. Each has its own template, folder, and purpose. Don't merge them or reuse one family's layout for another.

| Family | Folder | Template(s) | Format |
|---|---|---|---|
| **DSA practice** | `challenges/dsa/sessions/` | `dsa-session.md` | My existing Day-note format (task 5) |
| **Daily dev journal** | `daily/` | `daily.md` | New, defined below. Separate from DSA. |
| **Bugs** | `bugs/` | `bug.md` | New, defined below |
| **CBL challenges** | `challenges/<slug>/` + shared `concepts/`, `sources/` | `concept.md`, `question.md`, `source.md` | **Unchanged.** Exactly what the CBL commands already produce. |

**Create these in `cbl-wiki/templates/`:**
- `dsa-session.md`: matches my Day-note format exactly (see task 5).
- `daily.md`: for my work day, not DSA practice. Sections:
  - Worked on (tickets/tasks)
  - Learned (with `[[links]]` to concepts)
  - Blockers
  - Questions for my senior
  - Tomorrow
  - Add a "DSA today: [[DayN]]" line only as a link if I practiced; the DSA content stays in the session note.
- `bug.md`: Symptom · Environment · Steps to reproduce · Root cause · Fix (code) · Lesson · Related concepts (`[[links]]`) · Date
- `concept.md`, `question.md`, `source.md`: exact copies of the formats the CBL commands already write (look at existing notes and the command files). They're for when I make a CBL note by hand.
- `ios-handbook.md`: What it is · When to use · Swift example · Pitfalls · Apple docs link · Related
- `hub.md`: topic overview + sections of links

**Keep the CBL challenge format as it is.** I like it. Don't redesign the challenge, index, log, dashboard, presentation, concept, question, or source note formats, and don't change the CBL phases or command behavior. The only CBL changes allowed are:
- the folder moves and multi-challenge path handling (tasks 1 and 3)
- the topic tags (task 2)
- the two new `list`/`switch` commands

### 5. DSA practice logs
I log DSA practice as **one note per session/day** (`Day3.md` etc.), not one note per problem. Keep that habit. This is my real format:

````markdown
# Day 3 — Session Notes
**Date**: 2026-04-10

---

## Problem: 121. Best Time to Buy and Sell Stock

**Approach**: Greedy — single pass, track the minimum buy price and compute max profit at each step.

### Final Solution (Swift)

```swift
// solution code
```

---

## Review Summary

| | |
|---|---|
| ✅ What worked | ... |
| ⚠️ Watch out for | ... |
| 🧠 Pattern | Greedy (not Kadane's) — one pass, locally optimal choice at each step |
| ⏱️ Complexity | O(n) time, O(1) space |

---

## Key Learnings

- **Pattern**: ...
- **Space complexity intuition**: ...
- **Kadane's vs Greedy**: ...
````

**The `dsa-session.md` template** keeps this exact layout and these headings, and adds:
- Frontmatter:
  ```yaml
  type: dsa-session
  date: {{date}}
  tags: [dsa, <pattern tags e.g. greedy, two-pointers>]
  problems: [121]           # LeetCode numbers solved this session
  patterns: ["[[greedy]]"]
  ```
- Optional `**Link**:` and `**Difficulty**:` lines under each `## Problem:` heading
- A `[[pattern]]` link in the 🧠 Pattern row (e.g. `[[greedy]]`, `[[kadanes-algorithm]]`)
- A `**Review again on**:` line at the end, for spaced repetition

**Import my existing session notes:**
- Ask me where my existing `Day*.md` files are, then move them into `challenges/dsa/sessions/`.
- For each one, add the frontmatter above (fill in `problems` and `patterns` from the content) and turn pattern mentions into `[[links]]`.
- **Don't change my code, review tables, or learnings text.** Only add links and frontmatter.

**Pattern concept notes:**
- Create a shared concept note in `concepts/` for each pattern or idea that appears in my sessions (e.g. `greedy.md`, `kadanes-algorithm.md`, `space-complexity.md`), tagged `dsa`.
- Each pattern note gets: what the pattern is, when to recognize it, a template/skeleton in Swift, and a **Problems** section linking back to the session notes where I used it.

**DSA hub (`maps/dsa.md`):**
- List patterns, and under each pattern list the problems I solved (`121. Best Time to Buy and Sell Stock — [[Day3]]`).
- Check that every link resolves.

### 6. Obsidian config (in `cbl-wiki/.obsidian/`)
- Turn on the core plugins **Templates** and **Daily notes** (edit `core-plugins.json`; keep its existing format).
- Templates folder: `templates`. Daily notes: folder `daily`, format `YYYY-MM-DD`, template `templates/daily`.
- `graph.json`: replace the path-based color groups with tag-based ones (`tag:#database`, `tag:#system-design`, `tag:#distributed-systems`, `tag:#dsa`, `tag:#ios`, and `path:bugs`). Pick clearly different colors. Update the search filter so it hides `daily/`, `templates/`, and each challenge's `index`/`log` files.
- Keep the other existing settings.

### 7. Hub notes in `maps/`
- Create `system-design.md`, `databases.md`, `distributed-systems.md`, `dsa.md`, and `ios.md`.
- Fill the first three with links to the existing concepts by topic. Build `dsa.md` as described in task 5, and add empty sections for patterns I haven't practiced yet (two pointers, sliding window, BFS/DFS, DP…). Leave `ios.md` as a starter outline (Swift basics, SwiftUI, UIKit, concurrency, networking, persistence, testing, performance).
- Add a `home.md` in the vault root linking to all hubs, active challenges, today's daily note, and the inbox.

### 8. Housekeeping
- Add `cbl-wiki/.obsidian/plugins/lean-terminal/shell-integration/.zsh_history` to `.gitignore` and `git rm --cached` it. The repo is public.

## How to work
1. Plan first: show me the full list of moves, the tag table, and the command changes. **Wait for my OK** before changing files.
2. Then do it in small, logical commits (move → frontmatter → commands → templates → DSA import → config → hubs).
3. Verify before you finish:
   - Write a quick script that finds all `[[wikilinks]]` and reports any that don't resolve to a file. There should be zero new broken links.
   - Grep `.claude/` and `CLAUDE.md` for old paths (`cbl-wiki/questions/`, `cbl-wiki/challenge.md`, `cbl-wiki/log.md`, `cbl-wiki/index.md`, root `cbl-session.md`). There should be none left.
   - Confirm every `.json` file in `.obsidian/` still parses.
4. Don't push or open a PR until I say so. End with a short summary of what changed and anything I need to do in Obsidian (e.g. reload the vault, then on the tablet let it sync).
