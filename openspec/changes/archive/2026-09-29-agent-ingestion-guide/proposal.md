# Proposal

## Why

The site exists so that agents can turn a raw, half-certain message ("75 inches to the back of the (irremovable?) second row, I think the Limited is a factor") into a durable, checked fact. Nothing in the repo tells an agent how to do that today: the README covers the file contract but not sanity-checking, fact-checking, or provenance. An agent dropped into this repo with such a message has to guess.

## What Changes

- Add `AGENTS.md`: the agent-facing procedure for ingesting a raw tidbit into an entry (extract claims, sanity-check, fact-check, write, commit).
- Add `CLAUDE.md` containing only an import of `AGENTS.md`, so Claude Code sessions load it automatically. Other agents read `AGENTS.md` directly.
- Point the README at `AGENTS.md` for the ingestion procedure.
- Extend the entry contract (format 2): a versioned marker, a `Human` section holding only verbatim words with a signature, a `Robot` section holding analysis and provenance signed with the AI make and model, and a closing rule. Existing entries are not migrated.
- Prove the guide works with a cold test: a fresh agent given only a raw message, no other context, must produce a correct committed entry.

## Capabilities

### New Capabilities
- `entry-ingestion`: how a raw submission becomes an entry: claims preserved as reported, uncertainty resolved or flagged, nothing invented, private data excluded, result committed.

### Modified Capabilities
- `entry-format`: adds a requirement that every entry states its provenance. Existing requirements are unchanged.

## Impact

- New files `AGENTS.md`, `CLAUDE.md`; README gains one pointer.
- Existing entries stay in format 1; the marker and the closing rule make the two eras visible on the page.
- No pipeline, workflow, or hosting change.
