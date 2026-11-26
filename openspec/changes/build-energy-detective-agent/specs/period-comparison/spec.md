## Purpose

Compare consumption between two periods under explicit, comparable conditions, including per-sub-meter differences.

## ADDED Requirements

### Requirement: Absolute difference with stated baseline
The system SHALL report the absolute difference in kWh between two periods and SHALL state which period is the baseline.

#### Scenario: Day-over-day comparison
- **WHEN** the user compares yesterday with the day before
- **THEN** the answer gives the kWh difference and names the earlier day as the baseline

### Requirement: Percentage change rules
The system SHALL report a percentage change only when the baseline is complete and non-zero; otherwise it SHALL report the absolute difference and mark the percentage as not applicable.

#### Scenario: Zero baseline
- **WHEN** the baseline energy is zero
- **THEN** no percentage is reported and the reason is stated

#### Scenario: Incomplete baseline
- **WHEN** the baseline day is PARTIAL_OBSERVED
- **THEN** no full-day percentage is reported and the limitation is stated

### Requirement: Sub-meter attribution limit
The system SHALL attribute a change only to the sub-metered circuit that shows it and SHALL NOT attribute it to a single appliance.

#### Scenario: Kitchen circuit increase
- **WHEN** sub-meter 1 shows the largest increase
- **THEN** the answer refers to the circuit group, may ask which appliances were used, and does not name one appliance as the cause
