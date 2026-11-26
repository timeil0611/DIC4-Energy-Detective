## Purpose

Flag hours that deviate strongly from comparable history as candidates worth inspecting, with a transparent reference set and without claiming a cause or fault.

## ADDED Requirements

### Requirement: Comparable reference set
The system SHALL build the reference set for a target hour from complete readings at the same hour and day type (weekday or weekend) within the preceding 56 days, using only data before the target hour.

#### Scenario: Reference built from history
- **WHEN** a target hour on a Tuesday at 19:00 is evaluated
- **THEN** the reference contains only complete weekday 19:00 readings from the 56 days before it

### Requirement: Modified z-score flag
The system SHALL compute M = 0.6745 (x - median) / MAD and SHALL flag a candidate when the absolute value of M exceeds 3.5, reporting the reference size, median, MAD, and score.

#### Scenario: High deviation
- **WHEN** the score of the target hour is 4.2
- **THEN** the hour is flagged as a candidate and the median, MAD, and reference size are returned

### Requirement: Insufficient reference
The system SHALL return a limitation instead of a score when the reference has fewer than 14 readings, MAD is zero, or the target hour is incomplete.

#### Scenario: MAD equals zero
- **WHEN** all reference readings are identical
- **THEN** no score is computed and the limitation is reported

### Requirement: Candidate is not a diagnosis
The system SHALL describe a flagged hour as a candidate for inspection and SHALL NOT describe it as waste, a fault, or a confirmed cause.

#### Scenario: User asks if something is broken
- **WHEN** a candidate is flagged and the user asks whether an appliance is broken
- **THEN** the answer states that the data cannot confirm a fault and suggests what further information is needed
