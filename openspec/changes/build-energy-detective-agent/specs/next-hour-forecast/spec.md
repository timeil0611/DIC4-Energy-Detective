## Purpose

Estimate the energy of the next hour from information available at the replay boundary, always alongside simple baselines so that model value can be judged.

## ADDED Requirements

### Requirement: Forecast target and labeling
The system SHALL forecast the energy of the interval [h, h+1) and SHALL label the result as an estimate with its method name, never as an observation.

#### Scenario: Forecast displayed
- **WHEN** a forecast is returned
- **THEN** the answer names the method, the target interval, and states that it is an estimate

### Requirement: Baselines reported
The system SHALL compute persistence (previous complete hour) and daily seasonal naive (same hour on the previous day) baselines whenever their inputs are available.

#### Scenario: Baselines alongside model
- **WHEN** the Random Forest and both baselines can be computed
- **THEN** all three estimates are available in the evidence record

### Requirement: Training cut-off
The system SHALL use only a model whose training data end no later than the replay boundary and SHALL report that cut-off.

#### Scenario: No eligible model
- **WHEN** the only trained model has a training cut-off after h
- **THEN** no model forecast is returned and the limitation is stated

### Requirement: Missing features
The system SHALL NOT produce a model forecast when a required feature period is incomplete, and MAY return an available baseline clearly labeled as a baseline.

#### Scenario: Lag-24 missing
- **WHEN** the hour h-24 is incomplete
- **THEN** no Random Forest forecast is produced and any returned value is labeled as the persistence baseline
