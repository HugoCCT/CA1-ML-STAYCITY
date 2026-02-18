# CA1 Outline — Staycity Dublin (Towels & Linens Allocation)

## 1. Organisation Overview (10%)
- Brief overview of Staycity as an aparthotel operator in Dublin.
- Operational context: daily housekeeping, laundry, floor-based distribution.

## 2. Problem Statement (Business Context)
- Current state: towels and linens are distributed “at random” across floors.
- Observed impact: shortages where needed, surplus where not needed, repeated trips, delays in room readiness.
- Root cause hypothesis: allocation should be driven by **check-outs** (main driver of linen/towel replacement).

## 3. Proposed ML System (High Level)
- Objective: predict next-day demand for:
  - Towels
  - Linens (sheets)
- Granularity: **per floor, per day**.
- Output format: `LOW / MEDIUM / HIGH` demand classes.

## 4. ML Approaches Comparison (Core marks)
### 4.1 Supervised vs Unsupervised (in this context)
- Supervised: labels exist once we record actual demand outcomes (or derive from usage thresholds).
- Unsupervised: useful to discover patterns (types of days/floors) but not a direct decision tool.

### 4.2 Classification vs Clustering (in this context)
- Classification: direct operational decision support (predict demand class).
- Clustering: segmentation and insight; requires extra interpretation/rules to convert clusters into quantities.

## 5. Selected Approach + Rationale (30%)
- Choose supervised classification as primary.
- Explain why it is operationally actionable and measurable.
- Mention clustering as secondary support for insights.

## 6. Evaluation & Practical Deployment
- Metrics: F1-score for `HIGH` demand, confusion matrix, cost-sensitive evaluation.
- Deployment workflow: baseline rules → data capture → supervised model.
- Risks/limitations: data quality, seasonality/events, process change management.

## 7. References (Harvard)
- Staycity website/company pages, privacy statement (if referenced), general ML references.