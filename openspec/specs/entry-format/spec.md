# entry-format Specification

## Purpose
Defines the single contract an agent or human follows to add one minutia to the collection, so that any writer with repository access can publish without further coordination.

## Requirements

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

### Requirement: Entry states provenance
Every entry MUST end with a provenance line giving the as-of date and how the facts were established: measured or observed by the submitter, confirmed against a named source, or unverified. Claims with different provenance MUST be distinguishable to the reader.

#### Scenario: Measured plus confirmed
- **WHEN** an entry mixes a submitter's own measurements with a fact confirmed from a public source
- **THEN** the provenance line names the measurement as the submitter's and names the source for the confirmed fact, with a date

#### Scenario: Unverified claim
- **WHEN** a claim could not be confirmed
- **THEN** the entry marks that claim as unverified rather than omitting the marker or presenting it as fact

### Requirement: Human words are separated from robot analysis
Every entry MUST contain a `Human` section followed by a `Robot` section. The Human section SHALL contain only what people actually said: the original submission verbatim, and each follow-up answer verbatim, each answer distinguishable from the question that prompted it. It SHALL contain no summary or commentary and SHALL end with a signature line identifying the submitter. The Robot section SHALL contain the analysis and provenance and SHALL end with a signature line naming the AI make and model that produced it.

#### Scenario: Submission with one answered question
- **WHEN** the submitter answers one of several questions
- **THEN** the Human section shows the submission, the question, and the answer verbatim, and says nothing about the questions that received no answer

#### Scenario: Signatures present
- **WHEN** an entry is published
- **THEN** the Human section ends with the submitter's signature and the Robot section ends with the AI make and model

### Requirement: Entry format is versioned and never migrated
Every entry MUST carry a marker naming the entry format version it follows. When the format changes, existing entries SHALL be left as written, and each entry SHALL end with a visible rule so the page shows the boundary between files. Each format change SHALL add a divider file under `entries/`, named so it sorts between the last entry of the old format and the first of the new, that renders as a heading and a sentence stating what changed, so the page shows where the new generation begins.

#### Scenario: Format changes after entries exist
- **WHEN** the entry format moves to a new version
- **THEN** earlier entries keep their marker and content unchanged, and new entries carry the new marker

#### Scenario: Divider marks the generation change
- **WHEN** a reader scrolls from the last entry of the old format to the first of the new
- **THEN** a heading and a sentence between them state that the format changed and how
