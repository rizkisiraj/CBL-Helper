# CBL: Switch Active Challenge

Change which CBL challenge the other commands operate on.

## Context

The user's input: `$ARGUMENTS` — expected to be a challenge slug (e.g. `dsa`).

## What to Do

1. If `$ARGUMENTS` is empty, run `/project:cbl:list`'s logic to show the available
   slugs and ask the user to pick one.
2. Verify `cbl-wiki/challenges/<slug>/` exists. If not, tell the user it doesn't exist
   and show the available slugs (same listing as `/project:cbl:list`) — don't guess a
   close match silently.
3. Overwrite the root `.cbl-active` file with `<slug>` (just the slug, one line, no
   trailing content beyond a newline).
4. Confirm to the user: the active challenge is now `<slug>`, and briefly state its
   Mode and current phase (read from its `session.md`) so they know where they left off.

## Tone

Brief. This is a one-line context switch, not a conversation.
