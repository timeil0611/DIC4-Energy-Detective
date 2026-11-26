## Context

See proposal.md (Why). The repository currently holds design documents only (DESIGN.md, docs/); no application code, trained model, or experiment exists. The source dataset is the UCI single-household minute-level record (2006-2010). The architecture has eight numbered modules: (1) query and replay setting, (2) planning agent, (3) data access and quality check, (4) analytics and forecast tools, (5) energy knowledge base, (6) evidence registry, (7) response validator, (8) Streamlit presentation.

## Goals / Non-Goals

**Goals:**
- Make every number in an answer reproducible from a logged tool result.
- Enforce data rules (time boundary, units, completeness) in the tool layer, not through prompting.
- Keep the agent loop bounded and terminating.
- Support a controlled comparison with and without the external validator.

**Non-Goals:**
- Multi-agent orchestration or vector-database retrieval in this version.
- Imputation of missing data.
- Optimising forecasting accuracy beyond a single candidate model.

## Decisions

1. **Single planning agent over multi-agent.** One cloud LLM with structured tool calling handles interpretation, clarification, and coordination. Alternative: separate planner/analyst/critic agents. Rejected for this stage because it adds coordination cost and makes the A/B evaluation harder to attribute.

2. **Tools compute, LLM explains.** All totals, differences, percentages, scores, and forecasts come from deterministic tools. Alternative: let the LLM generate analysis code (as in prior energy agents). Rejected because generated code is harder to reproduce and audit.

3. **Historical replay with a fixed boundary h.** Each task freezes h and a read-only data view; tools refuse arguments that change them. Alternative: live data. Rejected because no sensor integration is in scope and replay makes answers reproducible.

4. **No imputation; complete hour = 60 valid minutes.** Partial intervals are reported as PARTIAL_OBSERVED with coverage. Alternative: interpolation. Rejected for the first version because imputed values could be mistaken for measurements; an imputation sensitivity study is future work.

5. **Random Forest with two naive baselines.** Initial setting: 200 trees, max_depth 12, min_samples_leaf 5, random_state 42; features: lags 1/2/24/168 h, 3 h and 24 h mean/std, calendar features. Chronological 70/15/15 split, one-step rolling replay on the test period. These are initial settings, not tuned conclusions. Alternative: gradient boosting or deep models. Deferred; the question first is whether any model beats the baselines.

6. **Median/MAD anomaly candidates.** Same hour and day type over the previous 56 days, at least 14 readings, |M| > 3.5 (initial threshold; sensitivity at 2.5/3.5/4.5). Alternative: model-residual or isolation-forest detectors. Rejected because the dataset has no fault labels and MAD gives an explainable reference.

7. **Small curated knowledge base (about 10 entries) with keyword/concept retrieval.** Alternative: RAG over a large corpus with a vector store. Deferred; a small sourced set is easier to verify and cite correctly.

8. **Evidence registry with result IDs; claims must reference IDs.** Alternative: post-hoc citation matching. Rejected because automatic matching can attach plausible but wrong evidence.

9. **External validator with budgets of 8 tool calls and 1 repair, then safe terminal reply.** Alternative: unlimited self-refinement or LLM self-critique. Rejected because unbounded loops raise cost and self-critique alone is not independent of the generator. The same budgets apply in both evaluation conditions.

## Risks / Trade-offs

- [Single old French household may not generalise] → State scope in every evaluation report; no claims about Taiwan or other homes.
- [Excluding incomplete hours reduces usable data] → Report exclusion rates; do not change rules after seeing test results.
- [Rule-based checks cannot judge free-text reasoning] → Two team members rate recommendations independently (0-2 on evidence support, inference boundary, actionability).
- [Validator may increase refusals and latency] → Report unnecessary-refusal rate, latency, tool calls, and repairs for both conditions.
- [Cloud LLM versions change] → Pin model ID and generation parameters before formal evaluation and record them.
- [Thresholds (3.5, 56 days, 14 readings, budget 8) are unvalidated] → Freeze before the formal run; document any change made during the pilot.

## Migration Plan

Not applicable (new system). Credentials are read from environment variables and never committed; raw data and model files are cached locally and excluded from version control.

## Open Questions

- Which LLM provider and pinned model ID will be used (depends on availability and cost at implementation time; does not change the specs).
