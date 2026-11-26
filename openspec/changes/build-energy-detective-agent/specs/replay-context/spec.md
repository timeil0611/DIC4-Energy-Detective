## Purpose

Fix the moment in history from which every task is answered, so that data, features, and models never use information that would not have been available at that time.

## ADDED Requirements

### Requirement: Fixed replay boundary
The system SHALL answer each task at a fixed replay boundary h, defined as an hour boundary on the dataset's original clock, and SHALL expose only records with timestamps strictly earlier than h.

#### Scenario: Records after the boundary are hidden
- **WHEN** the replay boundary is 14:00 and a tool reads data
- **THEN** no record with a timestamp at or after 14:00 is returned

#### Scenario: Agent cannot move the boundary
- **WHEN** the agent passes a different replay time or data view as a tool argument
- **THEN** the tool rejects the request and the rejection is recorded as evidence

### Requirement: Relative time interpretation
The system SHALL interpret relative expressions such as "today" and "yesterday" with respect to the replay date, and SHALL state the resolved interval in the answer.

#### Scenario: Yesterday resolved from replay date
- **WHEN** the replay boundary is 2008-03-12 14:00 and the user asks about yesterday
- **THEN** the analysed interval is 2008-03-11 00:00 to 2008-03-12 00:00 and the answer shows that interval

#### Scenario: Today is partial
- **WHEN** the user asks about today
- **THEN** the answer states that only the part of today before the replay boundary is covered

### Requirement: Original clock is preserved
The system SHALL use the dataset's original clock and SHALL NOT convert timestamps to another time zone.

#### Scenario: No time-zone conversion
- **WHEN** an interval is displayed
- **THEN** its timestamps match the original dataset clock
