## Purpose

Record every tool result with enough context to audit it, and bind each claim in an answer to the evidence that supports it.

## ADDED Requirements

### Requirement: Evidence record contents
The system SHALL record for each tool call the data version, replay time, interval, fields, quality status, tool or model version, parameters, result value and unit, status, and a unique result ID.

#### Scenario: Successful summary logged
- **WHEN** an energy summary succeeds
- **THEN** an evidence record with all required fields and a result ID exists

#### Scenario: Failure logged
- **WHEN** a tool call fails or is rejected
- **THEN** an evidence record with the failure status and reason exists

### Requirement: Claims bound to evidence IDs
The system SHALL accept a draft claim, number, or chart only if it references an existing result ID, and SHALL NOT attach a citation automatically to unsupported text.

#### Scenario: Unreferenced number
- **WHEN** a draft contains a number with no result ID
- **THEN** binding reports the number as unsupported

### Requirement: Reasoning is not evidence
The system SHALL NOT treat the model's internal reasoning as evidence.

#### Scenario: Evidence export
- **WHEN** the evidence for an answer is shown
- **THEN** it contains tool inputs and outputs and knowledge sources only
