## Purpose

Define when minute-level records count as valid and complete, so that partial or conflicting data is never presented as a complete measured total.

## ADDED Requirements

### Requirement: Missing values are not zero
The system SHALL NOT treat missing, non-numeric, or conflicting values as zero and SHALL NOT impute them in this version.

#### Scenario: Missing minute
- **WHEN** a minute in the requested interval has no valid value
- **THEN** that minute contributes nothing to the total and is counted as missing in the coverage

#### Scenario: Empty interval
- **WHEN** an interval has no valid observations
- **THEN** the system returns a LIMITED result instead of reporting zero consumption

### Requirement: Complete hour definition
The system SHALL label an hour as complete for a field only when all 60 distinct minutes on the original clock grid hold exactly one valid value for that field.

#### Scenario: 59 valid minutes
- **WHEN** an hour has 59 valid minutes for the requested field
- **THEN** the hour is labeled PARTIAL_OBSERVED with coverage 59/60

### Requirement: Duplicate and conflicting records
The system SHALL remove exact duplicate records and SHALL mark timestamps with conflicting values as invalid without choosing one of them.

#### Scenario: Exact duplicate
- **WHEN** two records share a timestamp and identical values
- **THEN** one record is kept and the deduplication is recorded

#### Scenario: Conflicting duplicate
- **WHEN** two records share a timestamp but differ in value
- **THEN** that minute is marked invalid and reported as a conflict

### Requirement: Field-specific checks
The system SHALL evaluate completeness separately for each requested field, so that a missing value in an unused field does not invalidate a requested field.

#### Scenario: Unused field missing
- **WHEN** voltage is missing but global active power is complete for an hour
- **THEN** a global active power query for that hour is complete
