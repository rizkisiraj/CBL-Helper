# CBL: List Challenges

List every CBL challenge in the vault, with its mode, phase, and progress, and mark
which one is active.

## Context

Read the root `.cbl-active` file to get the active challenge slug (if it exists).

## What to Do

1. List every folder under `cbl-wiki/challenges/`. For each, read its `session.md` to
   get:
   - **Mode** (Learn / Build)
   - **Current phase** (Engage / Investigate [+ cycle, Build mode] / Act / Evaluate / Complete)
   - **Progress** — for Investigate: `X/Y` guiding questions answered (current cycle, if
     Build); for Act: `X/Y` implementation steps complete
2. Present as a table:

```
Slug                  Mode   Phase                Progress        Active
---------------------------------------------------------------------------
db-stress-testing      Learn  Investigate          25/52 Qs         ●
dsa                    —      —                    —                
```

Mark the row matching `.cbl-active` with `●` in the Active column. If `.cbl-active` is
missing or points to a slug that doesn't exist, say so and suggest
`/project:cbl:switch <slug>`.

3. If `cbl-wiki/challenges/` has no folders yet, tell the user no challenges exist and
   suggest `/project:cbl:start`.

## Tone

Brief and clear. This is a dashboard, not a conversation.
