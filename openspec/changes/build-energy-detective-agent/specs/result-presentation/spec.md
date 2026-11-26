## Purpose

Present answers with a clear status, the interval and method used, charts tied to evidence, and the limitations of the data.

## ADDED Requirements

### Requirement: Response status
The system SHALL label every response with one of ANSWER, CLARIFY, PARTIAL, or LIMITED, and SHALL NOT present the status as a confidence score.

#### Scenario: Partial answer
- **WHEN** only some requested results can be supported
- **THEN** the response is labeled PARTIAL and lists what was not completed

### Requirement: Observation, estimate, and hypothesis separated
The system SHALL present historical observations, model estimates, and possible causes as separately labeled categories.

#### Scenario: Mixed answer
- **WHEN** an answer contains a measured total, a forecast, and a suggestion
- **THEN** each part is shown under its own label

### Requirement: Evidence and limits on demand
The interface SHALL let the user expand each answer to see the interval, method, data coverage, evidence IDs, and sources.

#### Scenario: Expand evidence
- **WHEN** the user expands the evidence panel
- **THEN** the interval, method, coverage, and source of each number are shown

### Requirement: No savings claims
The system SHALL NOT claim that a recommendation will save a specific amount of energy, cost, or carbon.

#### Scenario: Recommendation shown
- **WHEN** a recommendation is shown
- **THEN** it is phrased as an action to check and states that savings require separate verification
