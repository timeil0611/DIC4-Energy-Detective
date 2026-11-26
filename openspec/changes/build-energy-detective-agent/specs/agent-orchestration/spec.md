## Purpose

Define how the planning agent interprets requests, asks for clarification, and coordinates tools within a bounded budget, while leaving all computation to tools.

## ADDED Requirements

### Requirement: Numbers come from tools
The agent SHALL obtain every quantity in an answer from a tool result and SHALL NOT compute totals, differences, or percentages in free text.

#### Scenario: Difference requested
- **WHEN** the user asks how much more energy was used
- **THEN** the number in the answer equals the value returned by the comparison tool

### Requirement: Clarification for ambiguous requests
The agent SHALL ask a clarification question and end the current task when a request lacks information needed to choose an interval, field, or task type.

#### Scenario: Ambiguous period
- **WHEN** the user asks "was last week high?" without a comparison basis
- **THEN** the system returns a CLARIFY response naming the missing information

### Requirement: Bounded tool budget
The agent SHALL execute at most 8 agent-initiated tool calls per task, counting failed and rejected calls, and the budget SHALL NOT reset after a repair.

#### Scenario: Budget exhausted
- **WHEN** a task has used 8 tool calls and the agent requests another tool
- **THEN** the system returns a LIMITED reply built only from recorded evidence

### Requirement: Invalid actions terminate safely
The agent SHALL end the task with a safe terminal reply when the model returns an invalid action or the model service fails.

#### Scenario: Model service error
- **WHEN** the LLM API call fails
- **THEN** the user receives a LIMITED reply stating the service problem and any confirmed results
