## Purpose

Provide a small, curated set of sourced energy knowledge entries that the agent can cite, each with explicit limits on what may be inferred.

## ADDED Requirements

### Requirement: Entry structure
The knowledge base SHALL store each entry with an ID, definition, applicability conditions, statements that cannot be inferred, and an original source.

#### Scenario: Entry retrieved
- **WHEN** an entry is retrieved
- **THEN** its ID, rule, applicability limits, and source are returned together

### Requirement: Explicit no-match
The system SHALL report that no entry matched when retrieval finds nothing and SHALL NOT fabricate a citation.

#### Scenario: Unknown concept
- **WHEN** the agent queries a concept with no matching entry
- **THEN** the tool returns an explicit no-match result and the answer cites no knowledge entry for it

### Requirement: Knowledge does not prove a cause
The system SHALL NOT use a general knowledge entry as evidence that a specific appliance or behavior caused an observed change.

#### Scenario: General rule cited
- **WHEN** an entry about heating loads is cited next to an increase on a sub-meter
- **THEN** the answer presents it as a possible explanation to check, not as the cause
