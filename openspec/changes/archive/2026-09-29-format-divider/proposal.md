# Proposal

## Why

Format 2 entries render after the format 1 entries with nothing on the page announcing that the shape changed. A reader scrolling from the old entries into the new ones should see where the generation changed and why, without the page generator learning anything about formats.

## What Changes

- Add a divider file under `entries/` that sorts between the last format 1 entry and the first format 2 entry and renders as a heading, one explanatory sentence, and rules above and below.
- Name it `YYYY-MM-DD-0-format-N.md`; the `0` sorts it before any entry dated the same day.
- Record the convention in `AGENTS.md` so the next format change adds its own divider.

## Capabilities

### New Capabilities
_None._

### Modified Capabilities
- `entry-format`: the "versioned and never migrated" requirement gains the divider file as the visible boundary between generations.

## Impact

- One new file in `entries/`, one paragraph in `AGENTS.md`. No workflow or hosting change.
