## Purpose

Provide energy totals for requested intervals with correct units, coverage, and completeness labels, computed by tools rather than by the language model.

## ADDED Requirements

### Requirement: Unit conversion to kWh
The system SHALL convert global active power (kW, one-minute average) to energy as the sum of P/60, and sub-meter energy (Wh per minute) as the sum divided by 1000, reporting both in kWh.

#### Scenario: Global active power
- **WHEN** 60 valid minutes each have global active power 1.2 kW
- **THEN** the hourly energy is reported as 1.2 kWh

#### Scenario: Sub-meter energy
- **WHEN** 60 valid minutes each have sub-meter energy 10 Wh
- **THEN** the hourly energy is reported as 0.6 kWh

### Requirement: Unsupported fields
The system SHALL refuse to sum fields that are not energy or power, such as voltage or current, and SHALL return a LIMITED result naming the field.

#### Scenario: Voltage sum requested
- **WHEN** a summary of voltage energy is requested
- **THEN** the result is LIMITED with reason FIELD_NOT_SUPPORTED_FOR_ENERGY_SUM

### Requirement: Coverage and completeness reported
The system SHALL return each total with its coverage ratio and a COMPLETE_OBSERVED or PARTIAL_OBSERVED label.

#### Scenario: Partial day
- **WHEN** a requested day has missing minutes
- **THEN** the result is labeled PARTIAL_OBSERVED and the answer does not call it a full-day total

### Requirement: Peak hour definition
The system SHALL define the highest-usage period as the hour with the largest energy and SHALL NOT present it as instantaneous peak power.

#### Scenario: Peak question
- **WHEN** the user asks for the peak period of a day
- **THEN** the answer names the hour with the highest kWh and states that it is an hourly energy peak
