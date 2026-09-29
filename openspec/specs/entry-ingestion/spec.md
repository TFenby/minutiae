# entry-ingestion Specification

## Purpose
Defines how a raw submission (a chat message, a note, a photo caption) becomes a published entry, so that any agent given only the message and this repository produces the same kind of result.

## Requirements

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

### Requirement: Unstated conditions are asked about before writing
When a submitted value depends on a condition the submission does not state (position, configuration, version, endpoints), or two values contradict each other, the agent SHALL ask the submitter and wait before writing. Only when no one can answer SHALL the agent write, and then the entry MUST say the condition was not stated. Units and internal consistency SHALL be checked, and a contradiction SHALL never be silently corrected.

#### Scenario: Measurement depends on an unstated position
- **WHEN** a length is measured to a movable seat and the message does not say where the seat was
- **THEN** the agent asks where the seat was before writing the entry, and the entry records the answer beside the number

#### Scenario: Nobody can answer
- **WHEN** the agent asks and receives no answer because it is running unattended
- **THEN** the entry records the value with the condition marked as not stated

#### Scenario: Contradictory measurements
- **WHEN** two submitted values cannot both be true as described
- **THEN** the agent asks the submitter, or failing that records both with a note of the conflict

### Requirement: Sources are primary and claims match them
Fact-checks SHALL prefer primary sources (manufacturer, official documentation, maintainer changelogs). A secondary source SHALL be identified as such in the entry. The entry SHALL claim only what the cited source states, and every number not supplied by the submitter SHALL carry a source.

#### Scenario: Source says less than the claim
- **WHEN** a source says a feature is available on a grade and the agent would otherwise write that it is standard
- **THEN** the entry says available, matching the source

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
