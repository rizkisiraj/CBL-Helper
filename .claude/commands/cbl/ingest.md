# CBL: Ingest raw sources into the Wiki

Compile the documents dropped in `cbl-wiki/raw/` into the wiki — faithful summaries,
extracted concepts, and backlinks. This is the "raw → compiled wiki" step of the
knowledge-base loop.

## Conventions

Read `.claude/cbl-wiki.md` for the vault layout, page formats, frontmatter, and linking
rules. The target vault is **this project's `cbl-wiki/`**; `raw/`, `sources/`, and
`concepts/` are shared across all challenges. Read the root `.cbl-active` file to get
the active challenge slug — new concepts link to that challenge's
`challenges/<slug>/questions/` where relevant.

Additional input: `$ARGUMENTS` — optional: a specific filename in `raw/`, or `all`
(default: every not-yet-compiled file).

## Steps

1. **List `cbl-wiki/raw/`.** Each file (PDF, `.md`, `.txt`, `.html`, image, `.csv`) is a
   source. **Skip already-compiled ones** — a source is already compiled if a
   `sources/<slug>.md` exists with a matching `raw_file:` field, or `log.md` records it.
   Process only new or changed files. This makes ingest **incremental**.

2. **For each raw document:**
   - **Read it** — `.md`/`.txt`/`.html`/`.csv` directly; PDFs and images via the Read
     tool. If it's a clipped web article, capture its source URL.
   - **Summarize → `sources/<slug>.md`** — a faithful summary of the document's own
     content (main claims, key facts), with frontmatter `type: source`,
     `raw_file: raw/<filename>`, and `url:` if present. Don't editorialize here.
   - **Extract concepts** — identify the distinct ideas it teaches. For each, **read
     existing `concepts/` first** and *extend/strengthen* the matching page rather than
     duplicating; only create a new page if none fits. Add `**Sources:** [[<source>]]`
     and a topic `tags:` field.
   - **Wire links** — connect new concepts to related existing `[[concepts]]`, and to any
     `[[questions]]` they answer in the active challenge's `questions/` folder (flip
     those question pages to `status: answered`).

3. **Maintain index + log** — update the active challenge's `index.md` (new pages,
   progress count) and append one dated line to its `log.md` per compiled source.

4. **Report** — which raw files were compiled, what pages were created vs extended, which
   questions got answered, and any docs too thin to extract cleanly. Suggest running
   `/project:cbl:lint` to tidy and fill gaps.

## Notes

- `raw/` is **immutable** — read it, never modify it. The wiki can be fully recompiled
  from `raw/` at any time.
- **Offline by default** — digest what's in `raw/`; don't web-search during ingest. If a
  source is too thin to extract a concept cleanly, note the gap so `/project:cbl:lint`
  can later impute it.
- A single rich document can legitimately produce one source page and several
  interlinked concept pages.
