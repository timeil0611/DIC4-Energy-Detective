## Purpose

Check each answer draft against recorded evidence before it is shown, allow one repair, and never show an unchecked draft.

## ADDED Requirements

### Requirement: Objective checks
The validator SHALL check numbers, units, dates, direction of change, chart IDs, and citations in a draft against the evidence registry.

#### Scenario: Wrong unit
- **WHEN** a draft reports a kWh value as kW
- **THEN** validation fails with a unit issue

#### Scenario: Wrong direction
- **WHEN** a draft says usage decreased but the evidence shows an increase
- **THEN** validation fails with a direction issue

### Requirement: Numeric tolerance
The validator SHALL accept a numeric match within 0.005 for values rounded to two decimals, plus a small floating-point tolerance, and SHALL still check dates and units separately.

#### Scenario: Rounded value
- **WHEN** the evidence value is 3.456 kWh and the draft says 3.46 kWh
- **THEN** the number check passes

### Requirement: Boundary violations
The validator SHALL fail a draft that attributes a sub-meter change to a single appliance or calls an anomaly candidate a fault.

#### Scenario: Appliance blamed
- **WHEN** a draft states that the dishwasher caused a sub-meter 1 increase
- **THEN** validation fails with an inference-boundary issue

### Requirement: One repair then safe terminal
The system SHALL allow at most one repair per task and SHALL return a fixed-structure LIMITED or PARTIAL reply containing only confirmed evidence if the repaired draft fails.

#### Scenario: Second failure
- **WHEN** a repaired draft fails validation
- **THEN** the unchecked draft is not shown and a safe terminal reply states the reason
