# Dataset Assumptions — Towels & Linens Allocation by Floor (Staycity Dublin)

## 1) Why we need a dataset
The current towel and linen distribution process is mostly ad-hoc (random allocation by floor), which leads to:
- shortages on floors that actually need stock,
- surplus on floors that do not need stock,
- repeated porter trips and operational delays.

To support a consistent and scalable process, we need a dataset that links:
**(a) demand drivers** (especially check-outs) to **(b) actual demand outcomes** (what was truly needed).

This enables either:
- an immediate rule-based baseline (check-outs → recommended packs), and later
- a supervised ML model that predicts demand levels by floor.

---

## 2) Prediction unit (what one “row” represents)
We define one row in the dataset as:

**(date, floor, item_type)**

Where:
- `date` = the day we are planning/preparing for (e.g., tomorrow)
- `floor` = floor number or floor identifier (e.g., Floor 3, Floor 4)
- `item_type` = `towels` or `linens` (sheets).  
  (Note: pillowcases and duvets are considered more standard; the main operational pain is towels and linens.)

Example rows:
- 2026-02-19 | Floor 3 | towels
- 2026-02-19 | Floor 3 | linens
- 2026-02-19 | Floor 4 | towels
- 2026-02-19 | Floor 4 | linens

This structure supports decision-making at the exact point of the problem: **how much to allocate per floor each day**.

---

## 3) Inputs (features) — what the model would use
### Primary driver (most important)
- **`checkouts_floor`**  
  The number of check-outs on that floor for the planning day.  
  Rationale: check-outs are the main trigger for towel replacement and bed/linen change.

### Optional supporting signals (if available)
These features improve accuracy but are not strictly required to start:

- **`occupied_rooms_floor`**  
  Total rooms occupied on the floor (proxy for general demand).

- **`checkins_floor`**  
  Helps anticipate fresh room setups (often correlates with towel/linen needs).

- **`avg_stay_length_floor`** (or proportion of long-stay guests)  
  Long-stay guests may not require full linen replacement daily, depending on policy.

- **`day_of_week`**  
  Weekends vs weekdays can affect turnover patterns.

- **`month` / `season`**  
  Captures seasonality (busy periods vs quieter months).

- **`special_event_flag`** (optional and high-level)  
  City events can impact occupancy and turnover (no guest personal data needed).

- **`urgent_topups_yesterday`** (per floor + item_type)  
  Count of urgent requests for extra packs the previous day.
  Rationale: frequent urgent requests indicate under-allocation patterns.

Important note:
This design intentionally avoids personal guest data. The dataset can be built using **aggregated operational counts** per floor.

---

## 4) Output (label/target) — what we want to predict
To satisfy operational simplicity and the module focus on classification, we define the prediction as:

**`demand_class` ∈ {LOW, MEDIUM, HIGH}**

We predict `demand_class` separately for:
- towels, and
- linens (sheets),
for each floor and date.

Why classification (LOW/MED/HIGH) is a good fit:
- It is easy for staff to understand and act on.
- It matches the real operational decision: “Do we send extra packs or not?”
- It works even when early data collection is imperfect.

---

## 5) How to create labels (because currently nothing is recorded)
Right now, there is no formal record of actual consumption by floor; managers rely on experience and do not consistently communicate to porters.  
Therefore, the dataset requires a simple logging process to create labels.

### Option A — Threshold labels (business-defined)
After collecting daily logs for a few weeks, define thresholds:
- **LOW**: below X packs used/requested
- **MEDIUM**: X to Y packs
- **HIGH**: above Y packs

Thresholds should be chosen with managers to reflect real stock movement.

### Option B — Percentile labels (data-driven)
If the distribution varies a lot, define classes based on history:
- LOW = bottom 33%
- MEDIUM = middle 33%
- HIGH = top 33%

This is useful when the property is highly seasonal.

---

## 6) What “actual demand” means (what we measure)
To create accurate labels, we need a consistent definition of “demand outcome”.

Recommended operational measure (practical):
- **`packs_delivered_floor_item`**: number of packs delivered to that floor for that item type  
AND
- **`urgent_topups_floor_item`**: additional packs requested later due to shortage

A floor-day with many urgent top-ups indicates the initial allocation was too low.

If packs are not currently tracked, the logging can start with simple counts (even approximate) and improve over time.

---

## 7) Data capture plan (minimal overhead)
### Phase 0 — Immediate baseline without ML (already an improvement)
Use a simple rule:
- allocate based on `checkouts_floor`  
Example: “each checkout requires N towel packs and N linen packs” (values set by management).

This eliminates randomness immediately.

### Phase 1 — Start logging (to enable supervised learning)
Create a lightweight daily spreadsheet or form with fields:
For each floor:
- check-outs
- packs delivered (towels)
- packs delivered (linens)
- urgent top-ups requested (towels)
- urgent top-ups requested (linens)

This can be filled by a manager or porter lead in a few minutes.

### Phase 2 — Train a supervised model
After 4–8 weeks of data, train a classifier to predict demand_class.
Initial models can be simple and interpretable (e.g., logistic regression, decision tree),
with performance measured using confusion matrix and F1-score for HIGH demand.

---

## 8) Assumptions and limitations (transparent for the report)
- Check-outs are assumed to be the strongest driver for towels and linens demand.
- Early logging may be noisy (human input, inconsistent counting), but improves with routine.
- Seasonality and event-driven spikes may require continuous monitoring and periodic recalibration.
- The proposed system is intended as decision support; staff can override recommendations when needed.