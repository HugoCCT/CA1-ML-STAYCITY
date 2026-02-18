# Metrics & Error Costs — Operational ML Evaluation

## Why error costs matter
Not all errors have the same operational impact:
- Predicting **LOW** when true demand is **HIGH** causes shortages, delays, and repeated porter trips.
- Predicting **HIGH** when true demand is **LOW** causes surplus and inefficient stock movement, but is usually less disruptive.

## Recommended evaluation metrics
- **Confusion Matrix** (to show where errors occur)
- **F1-score for the HIGH class** (focus on catching high-demand days)
- Precision/Recall trade-off:
  - High recall for HIGH demand reduces shortages
  - Precision ensures we don’t over-allocate too often

## Operational success criteria (examples)
- Reduction in urgent top-up requests
- Reduction in porter “repeat trips” for the same floor
- Improved room readiness / reduced delays
- More stable inventory usage and fewer “emergency” shortages