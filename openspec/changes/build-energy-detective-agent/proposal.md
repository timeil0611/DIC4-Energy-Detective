## Why

Household electricity records are long time series that most residents cannot interpret, and an LLM answering questions about them can still quote the wrong date, confuse power (kW) with energy (kWh), treat missing data as zero, or present a statistical deviation as an appliance fault. This change turns the Energy Detective design (DESIGN.md, docs/) into a verifiable contract so that implementation and evaluation can be checked against explicit, testable behavior.

## What Changes

- Introduce a single planning agent that interprets questions, asks for clarification, and coordinates tools; it never computes quantities itself.
- Introduce guarded analytics tools for energy summaries, period comparison, anomaly candidates, and next-hour forecasting, all operating at a fixed historical replay time.
- Introduce an evidence registry that links every number, chart, and recommendation to its data slice, method, and source.
- Introduce an external response validator with a bounded budget of 8 tool calls and 1 repair per task, and a safe terminal reply when the budget or validation fails.
- Introduce a small curated energy knowledge base (about 10 entries) with explicit inference limits.
- Introduce a Streamlit presentation layer that shows answers, charts, evidence, and limitations.

All outcomes of this change are **Expected**. No model has been trained and no experiment has been run; no accuracy, reliability gain, or energy saving is claimed.

### Out of scope

- Real-time sensing, device control, or appliance scheduling optimisation.
- Identifying faults or individual appliances from sub-metered circuits.
- Claims of real energy, cost, or carbon savings (these require separate intervention studies).
- Taiwanese tariffs or generalisation beyond the single UCI household.

## Capabilities

### New Capabilities

- `replay-context`: Fixed historical replay time and read-only data view; no look-ahead in data, features, or model training.
- `data-quality`: Completeness, deduplication, conflict and missing-value rules for minute-level records.
- `energy-summary`: Energy totals in kWh with correct unit conversion, coverage, and completeness labels.
- `period-comparison`: Differences between comparable periods, with percentage rules for incomplete or zero baselines.
- `anomaly-candidates`: Median/MAD-based flagging of unusual hours as candidates for inspection, not diagnoses.
- `next-hour-forecast`: Next-hour energy estimate from a Random Forest compared with persistence and daily seasonal naive baselines.
- `domain-knowledge`: Retrieval of curated energy knowledge entries with sources and inference limits.
- `agent-orchestration`: Planning agent behavior, clarification, and the bounded tool-call budget.
- `evidence-registry`: Recording of tool results and binding of answer claims to evidence IDs.
- `response-validation`: Checking drafts against evidence, one repair, and safe terminal replies.
- `result-presentation`: Response statuses and display of answers, charts, evidence, and limitations.

### Modified Capabilities

None (no existing specs).

## Impact

- New code (planned, not yet created): `app.py`, `src/agent.py`, `src/data_access.py`, `src/analytics.py`, `src/forecast.py`, `src/evidence.py`, `src/validation.py`, `knowledge/energy_rules.json`, `scripts/train_forecast.py`, `evaluation/`.
- Dependencies: a cloud LLM API with structured tool calling (provider and pinned model ID to be selected), scikit-learn, pandas, Streamlit.
- Data: UCI Individual Household Electric Power Consumption (DOI 10.24432/C58K54), downloaded and cached locally, not committed.
