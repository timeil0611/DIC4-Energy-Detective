## 1. Data access and quality (modules 1, 3)

- [ ] 1.1 Download and cache the UCI dataset with a recorded data version; verify columns, units, and row count against the dataset page (module 3)
- [ ] 1.2 Implement the replay context that freezes h and a read-only view; unit test that no record with timestamp >= h is returned (module 1)
- [ ] 1.3 Implement exact-duplicate removal and conflict marking; unit tests for duplicate and conflicting timestamps (module 3)
- [ ] 1.4 Implement per-field completeness on the original minute grid; unit test that 59 valid minutes give PARTIAL_OBSERVED with coverage 59/60 (module 3)
- [ ] 1.5 Implement rejection of tool arguments that change h or the view; test that the rejection is returned as a structured status (module 3)

## 2. Analytics tools (module 4)

- [ ] 2.1 Implement SummarizeEnergy with kW/60 and Wh/1000 conversions; unit tests using the 1.2 kWh and 0.6 kWh spec scenarios
- [ ] 2.2 Return LIMITED for unsupported fields and empty intervals; tests for voltage and no-data intervals
- [ ] 2.3 Implement ComparePeriods with baseline labeling and percentage rules; tests for zero and incomplete baselines
- [ ] 2.4 Implement FlagAnomalyCandidates (56 days, same hour and day type, >= 14 readings, |M| > 3.5); tests for flagging, MAD = 0, and too few readings
- [ ] 2.5 Report flag frequencies at thresholds 2.5/3.5/4.5 on the validation period; save a short sensitivity table in evaluation/

## 3. Forecasting (module 4)

- [ ] 3.1 Build the hourly table and features (lags 1/2/24/168, 3 h and 24 h mean/std, calendar); test that no feature uses data at or after h
- [ ] 3.2 Implement persistence and daily seasonal naive baselines; unit tests on a synthetic series
- [ ] 3.3 Train the Random Forest on the 70% training split with initial parameters; save the model with its training cut-off metadata
- [ ] 3.4 Implement ForecastNextHour with cut-off and missing-feature checks; test the "no eligible model" and "lag-24 missing" scenarios
- [ ] 3.5 Evaluate RF and baselines with MAE, RMSE, availability, and sample counts on common test hours; write results to results/forecast.csv (RQ1)

## 4. Knowledge base (module 5)

- [ ] 4.1 Convert docs/08_domain_knowledge.md into knowledge/energy_rules.json with ID, rule, conditions, non-inferences, and source; validate the schema with a test
- [ ] 4.2 Implement RetrieveDomainKnowledge with concept/keyword matching and explicit no-match; tests for hit and no-match

## 5. Evidence registry (module 6)

- [ ] 5.1 Implement evidence records with all required fields and unique result IDs; test that successful and failed calls are both logged
- [ ] 5.2 Implement draft-to-evidence binding that flags unreferenced numbers and charts; test with an unreferenced number

## 6. Agent orchestration (module 2)

- [ ] 6.1 Select the LLM provider and pin the model ID and generation parameters; record them in a config file read from environment variables
- [ ] 6.2 Define tool schemas for structured tool calling from the tool contracts; test that each schema round-trips a valid call
- [ ] 6.3 Implement the agent loop with CLARIFY / TOOL / DRAFT actions and the 8-call budget; test budget exhaustion with a mocked model
- [ ] 6.4 Implement safe terminal replies for invalid actions and service errors; test with a mocked API failure

## 7. Response validation (module 7)

- [ ] 7.1 Implement checks for numbers (0.005 tolerance), units, dates, and direction; tests using the spec scenarios
- [ ] 7.2 Implement inference-boundary checks (single-appliance attribution, candidate called a fault); tests with violating drafts
- [ ] 7.3 Implement the one-repair rule and fixed-structure terminal reply; test that a second failure never shows the draft

## 8. Presentation (module 8)

- [ ] 8.1 Build the Streamlit page with replay-time selection and chat input; manual check that the chosen h is shown with each answer
- [ ] 8.2 Show status labels and separate observation / estimate / hypothesis sections; manual check on one example per status
- [ ] 8.3 Add an expandable evidence panel (interval, method, coverage, IDs, sources) and charts bound to result IDs; manual check that chart values match evidence

## 9. Evaluation (RQ2, RQ3)

- [ ] 9.1 Write 10 pilot tasks and 50 formal tasks (5 types x 6 regular + 4 boundary) with expected answers; peer-review and freeze the formal set in evaluation/tasks/
- [ ] 9.2 Run condition A (no validator) and condition B (validator + 1 repair), 3 runs each (300 task executions); store all runs, not only the best, in results/
- [ ] 9.3 Score completion, correctness, evidence consistency, limitation handling, unnecessary refusals, two-rater recommendation scores, latency, and cost; produce the results table for the paper
