# Traceability: paper claims → OpenSpec capabilities

Specs live in `openspec/changes/build-energy-detective-agent/specs/`.

| Paper claim | Capability (spec) |
|---|---|
| P1 Measurement definition (power ≠ energy) | `energy-summary`, `response-validation` |
| P2 Data granularity (sub-meter ≠ appliance) | `period-comparison`, `response-validation` |
| P3 Anomaly ≠ cause | `anomaly-candidates`, `domain-knowledge` |
| P4 No look-ahead | `replay-context`, `next-hour-forecast` |
| P5 Effect needs separate validation | `result-presentation` |
| Missing data is not zero | `data-quality` |
| Defense 1: tool computation | `agent-orchestration`, `energy-summary`, `period-comparison` |
| Defense 2: evidence recording | `evidence-registry` |
| Defense 3: response checking (8 calls, 1 repair) | `agent-orchestration`, `response-validation` |
| Trade-off: social comparison vs. concrete guidance | `result-presentation` |
| Trade-off: forecast accuracy vs. transparency | `next-hour-forecast` |
| Trade-off: LLM autonomy vs. deterministic checking | `agent-orchestration`, `response-validation` |
| RQ1 forecasting vs. baselines | `next-hour-forecast`; tasks 3.5 |
| RQ2 task correctness | all tool specs; tasks 9.1–9.3 |
| RQ3 validator ablation (A vs. B) | `response-validation`; tasks 9.2–9.3 |
