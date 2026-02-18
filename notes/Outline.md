## 4.2 Classification vs Clustering (Staycity context)

### Classification — what it means here (recommended)
Classification is a supervised learning task where the model predicts a category (label).
For this operational problem, the categories are designed to match the daily allocation decision:

- `LOW` demand
- `MEDIUM` demand
- `HIGH` demand

We run classification **per floor** and **per item type** (towels and linens).

Example:
- Inputs: `checkouts_floor=18`, `day_of_week=Friday`, `recent_urgent_topups=3`
- Output: `towels_demand_class = HIGH` (Floor 4)

**Why classification is operationally strong**
- The output is immediately actionable for porters and housekeeping.
- It is easy to map classes to “par levels” (recommended stock levels):
  - LOW → send minimum packs
  - MEDIUM → send standard packs
  - HIGH → send extra packs
- It is easy to evaluate and improve:
  - confusion matrix shows where mistakes happen
  - F1-score for HIGH demand focuses on avoiding shortages

**How classification reduces the current issue**
Because allocation is currently “random”, classification introduces a consistent, data-driven rule:
- floors with many check-outs are predicted as higher demand,
- floors with low turnover are predicted as lower demand,
which prevents shortages on floors that need stock and reduces waste elsewhere.

---

### Clustering — what it means here (alternative / secondary)
Clustering is an unsupervised task where the algorithm groups observations into clusters without labels.
In this context, clustering could group:
- floor-days with similar occupancy/turnover patterns, or
- floors that behave similarly over time.

Example clusters:
- Cluster 1: “high turnover days” (many check-outs)
- Cluster 2: “stable long-stay pattern” (few check-outs, stable demand)
- Cluster 3: “mid turnover weekday pattern”

**Why clustering is less suitable as the primary solution**
- Clusters do not directly tell you “how many packs to allocate tomorrow”.
- The result needs interpretation and additional rules to convert clusters into quantities.
- Two clusters may still require different towel vs linen decisions.

**Where clustering can still add value**
- Insight: understand typical operational patterns across floors/days.
- Planning: identify which floors behave similarly and can share stocking policies.
- Anomaly detection: flag days that do not match usual clusters (potential special events or unusual turnover).

---

### Summary (in one line)
For daily towel/linen allocation decisions, classification is the best primary approach because it outputs a clear LOW/MED/HIGH demand label per floor, while clustering is better as a secondary tool for discovering patterns and supporting planning.