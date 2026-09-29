# Spec Delta

## ADDED Requirements

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
Every entry MUST carry a marker naming the entry format version it follows. When the format changes, existing entries SHALL be left as written, and each entry SHALL end with a visible rule so the page shows the boundary between files.

#### Scenario: Format changes after entries exist
- **WHEN** the entry format moves to a new version
- **THEN** earlier entries keep their marker and content unchanged, and new entries carry the new marker

