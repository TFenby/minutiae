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
