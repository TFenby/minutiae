# Spec Delta

## Purpose

Defines how a raw submission (a chat message, a note, a photo caption) becomes a published entry, so that any agent given only the message and this repository produces the same kind of result.

## ADDED Requirements

### Requirement: Submitter data is preserved
Measurements and observations reported by the submitter SHALL be recorded as reported. An agent MAY add a unit conversion alongside a value but SHALL NOT replace a submitted value with one found elsewhere.

#### Scenario: Web figure differs from submitted measurement
- **WHEN** a published specification disagrees with the submitter's own measurement
- **THEN** the entry keeps the submitter's number and MAY note the published figure as a separate, sourced fact

### Requirement: Uncertainty is resolved or flagged
Any claim the submitter marks as uncertain (a question mark, "I think", "maybe") SHALL be checked against a source. The entry SHALL state whether the claim was confirmed, corrected, or left unverified.

#### Scenario: Hunch corrected by a source
- **WHEN** the submitter guesses that a feature depends on a trim level and a source shows it applies to all trims
- **THEN** the entry states the sourced fact and does not repeat the guess as fact

#### Scenario: No source found
- **WHEN** no source confirms or refutes an uncertain claim
- **THEN** the entry keeps the claim, marked unverified

### Requirement: Plausibility check before writing
Before writing, the agent SHALL check units and internal consistency of the submitted numbers. A contradiction SHALL be flagged in the entry or raised with the submitter, never silently corrected.

#### Scenario: Consistent nested measurements
- **WHEN** one measurement is described as spanning a longer path than another and its value is larger
- **THEN** the agent proceeds without comment

#### Scenario: Contradictory measurements
- **WHEN** two submitted values cannot both be true as described
- **THEN** the entry records both with a note of the conflict, or the agent asks the submitter before writing

### Requirement: Nothing invented, nothing private
The entry SHALL contain only facts from the submission or from a source it names. It SHALL exclude personal data such as names, addresses, plates, account identifiers, and anything the submitter marked private.

#### Scenario: Gap in the submission
- **WHEN** the submission lacks a detail the agent would expect
- **THEN** the entry omits it or marks it unknown rather than filling it in

### Requirement: Result is a committed entry
The ingestion SHALL end with an entry file that satisfies `entry-format`, committed to the repository with a message naming the file.

#### Scenario: Cold agent completes ingestion
- **WHEN** an agent with no context beyond this repository's guide receives a raw message
- **THEN** a new file exists under `entries/` following the naming and content rules, and a commit containing only that file exists
