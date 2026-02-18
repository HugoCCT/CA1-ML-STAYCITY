## 4.1 Supervised vs Unsupervised Learning (Staycity context)

### Supervised learning — what it means here
In this use case, supervised learning means training a model using historical examples where the “correct outcome” is known.
Once the property starts recording daily outcomes by floor (packs delivered + urgent top-ups), we can create labels such as:
- `LOW`, `MEDIUM`, `HIGH` towel demand
- `LOW`, `MEDIUM`, `HIGH` linen demand

A supervised model learns a mapping from operational inputs (especially check-outs) to these labels.  
For example:
- Inputs: tomorrow’s check-outs on Floor 4, day-of-week, recent urgent top-ups
- Output: “Floor 4 towels demand = HIGH”

**Why supervised fits the business goal**
- The business needs a clear, actionable prediction for each floor each day.
- There is a measurable outcome once we start logging (“how much was actually needed”).
- We can evaluate performance objectively using a confusion matrix and F1-score (especially for HIGH demand).

**Typical supervised tasks in this scenario**
- Classification: predict LOW/MED/HIGH per floor and item type.
- (Future improvement) Regression: predict the exact number of packs.

---

### Unsupervised learning — what it means here
Unsupervised learning does not require labelled outcomes. Instead, it finds structure or patterns in the data on its own.
In this scenario, unsupervised learning could be used to group similar floors or similar days into “profiles” based on features like:
- occupancy level
- turnover patterns (check-outs)
- day-of-week / seasonality

Example outputs:
- Cluster A: “high turnover floors on weekends”
- Cluster B: “long-stay heavy floors with stable demand”

**Why unsupervised is less direct for daily allocation**
- It produces groups/profiles, not an explicit prediction of what to allocate tomorrow.
- It usually requires human interpretation to convert clusters into concrete quantities.
- It is more suited to insight and planning than daily operational decisions.

**Where unsupervised could still help (secondary use)**
- Identifying recurring operational patterns and exceptions (e.g., unusual days).
- Supporting managers with insight (e.g., “Floors 5–6 behave similarly, consider similar stock rules”).
- Highlighting anomalies (e.g., a day that does not match usual patterns).

---

### Summary (in one line)
For the towel/linen allocation problem, supervised learning is the best primary approach because it produces a measurable, actionable prediction per floor per day, while unsupervised learning is better as a secondary tool for discovering patterns and supporting managerial insight.