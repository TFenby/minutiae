# Spec Delta

## Purpose

Defines the single contract an agent or human follows to add one minutia to the collection, so that any writer with repository access can publish without further coordination.

## ADDED Requirements

### Requirement: One Markdown file per entry
Each minutia SHALL be a single Markdown file under `entries/` at the repository root. A writer SHALL add an entry by creating a new file and SHALL NOT need to modify any other file.

#### Scenario: Agent adds an entry
- **WHEN** a writer creates `entries/<name>.md` containing valid Markdown and pushes it to the default branch
- **THEN** the entry appears on the published page after the next build with no other change required

### Requirement: Entry file naming
Entry filenames MUST match `YYYY-MM-DD-<slug>.md`, where the date is the day the fact was recorded or last verified and `<slug>` is lowercase ASCII letters, digits and hyphens.

#### Scenario: Well-formed name sorts chronologically
- **WHEN** entries named `2024-03-10-signal-cli-nixos.md` and `2026-09-28-sienna-cargo.md` both exist
- **THEN** the 2024 entry appears before the 2026 entry on the published page

### Requirement: Entry content shape
The first line of an entry MUST be a Markdown heading naming the minutia. The remainder is free-form Markdown; fenced code blocks and links MUST render on the published page.

#### Scenario: Entry with a command
- **WHEN** an entry's body contains a fenced code block
- **THEN** the published page renders it as preformatted code with its contents unchanged
