# Metrics & Error Costs — Evaluating an Allocation Classifier (Staycity)

## 1) Why evaluation matters
The goal of this system is operational improvement:
- fewer shortages on floors that need towels/linens,
- fewer urgent “top-up” requests,
- fewer repeated porter trips,
- smoother housekeeping workflow.

Because the output is a demand class (`LOW/MEDIUM/HIGH`), evaluation should focus on:
- how often the model predicts the correct class,
- how expensive the mistakes are (some errors disrupt operations more than others).

---

## 2) Confusion matrix (the key operational tool)
A confusion matrix shows how predictions compare to reality.

Example interpretation:
- Predict **LOW** when the true class is **HIGH** → major operational risk (shortages, delays).
- Predict **HIGH** when the true class is **LOW** → inefficiency (extra packs moved), but usually less disruptive.

This makes the confusion matrix extremely useful for:
- spotting systematic under-allocation (dangerous),
- understanding whether the model is too conservative or too aggressive.

---

## 3) Recommended ML metrics
### Accuracy (basic)
Accuracy is easy to report but can be misleading if most days are MEDIUM.

### Precision and Recall (especially for HIGH demand)
- **Recall (HIGH)**: of all truly HIGH-demand cases, how many did we correctly detect?
  - High recall reduces shortages.
- **Precision (HIGH)**: of all predicted HIGH-demand cases, how many were truly HIGH?
  - Higher precision reduces waste from over-allocation.

### F1-score (HIGH) — recommended headline metric
The F1-score combines precision and recall for the HIGH class.
This is useful because HIGH demand is the most critical class for avoiding operational issues.

---

## 4) Cost-sensitive evaluation (business reality)
In operations, errors have different costs:
- **False LOW** (predict LOW, true HIGH) is the worst case:
  - missing stock on the floor,
  - urgent calls,
  - repeated porter trips,
  - delays in room readiness.
- **False HIGH** (predict HIGH, true LOW) is usually less harmful:
  - some wasted movement and surplus,
  - but service levels are protected.

Therefore, the system should be tuned to prefer:
- catching HIGH demand even if it slightly increases predicted HIGH cases,
as long as waste remains acceptable.

---

## 5) Operational KPIs (what managers care about)
Beyond ML metrics, the best proof of value is operational KPIs such as:
- number of urgent top-up requests per day (should decrease),
- number of repeated trips by porters to the same floor (should decrease),
- time to make rooms ready / delays related to missing linens (should decrease),
- stability of inventory usage (fewer emergency shortages).

These KPIs connect the model directly to business performance.

---

## 6) Monitoring and drift
Demand patterns change with:
- seasonality (busy months),
- city events,
- policy changes (e.g., linen change frequency),
- staffing changes.

The model should be monitored weekly or monthly:
- check confusion matrix trends,
- check HIGH-demand recall,
- recalibrate thresholds or retrain when performance drops.