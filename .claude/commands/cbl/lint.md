# CBL: Lint the Wiki (health check)

Run a health pass over this project's `cbl-wiki/` to keep it coherent, find gaps, and
surface new things worth investigating. Safe to run anytime; also invoked by
`/project:cbl:synthesize`.

## Conventions

Read `.claude/cbl-wiki.md` for the vault layout and conventions. Target vault =
`cbl-wiki/`. Input: `$ARGUMENTS` — optional focus (e.g. `links`, `contradictions`,
`enrich`).

## Checks

1. **Structural**
   - **Orphans** — pages with no inbound links; propose links or flag.
   - **Broken links** — `[[slug]]` with no matching file; create a stub or fix the link.
   - **Coverage** — answered questions with no linked concept; concepts with no source;
     open questions that an existing concept actually already answers.

2. **Integrity**
   - **Contradictions / inconsistencies** between concept pages — flag both pages and
     note in `log.md`; don't silently "fix."
   - **Near-duplicate concepts** (similar slugs/topics) — propose a merge.

3. **Enrichment** (suggest first, act on approval)
   - **Impute missing/weak data** — list thin concepts or unresolved gaps and **offer**
     to fill them via web search (WebSearch / WebFetch). Only search after the user
     confirms, or if `$ARGUMENTS` contains `enrich`/`impute`.
   - **New article candidates** — recurring mentions that deserve their own concept page.
   - **Follow-up questions** — interesting threads worth investigating; add the good ones
     as `questions/` stubs (`status: open`) if the user wants.

## Output

- Apply the **safe** fixes automatically (create missing stubs, add obvious links, log
  notes).
- Present the **judgment calls** (contradictions, merges, enrichment, new questions) as a
  short list for the user to approve.
- Append a dated summary to `log.md`.

## Notes

- Default to **offline**; web search only for enrichment and only with consent.
- Be conservative — **never delete content**; flag and propose instead. A wrong "fact"
  can propagate across linked pages, so prioritize contradiction-spotting.
