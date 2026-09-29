# Spec Delta

## MODIFIED Requirements

### Requirement: Entry format is versioned and never migrated
Every entry MUST carry a marker naming the entry format version it follows. When the format changes, existing entries SHALL be left as written, and each entry SHALL end with a visible rule so the page shows the boundary between files. Each format change SHALL add a divider file under `entries/`, named so it sorts between the last entry of the old format and the first of the new, that renders as a heading and a sentence stating what changed, so the page shows where the new generation begins.

#### Scenario: Format changes after entries exist
- **WHEN** the entry format moves to a new version
- **THEN** earlier entries keep their marker and content unchanged, and new entries carry the new marker

#### Scenario: Divider marks the generation change
- **WHEN** a reader scrolls from the last entry of the old format to the first of the new
- **THEN** a heading and a sentence between them state that the format changed and how
