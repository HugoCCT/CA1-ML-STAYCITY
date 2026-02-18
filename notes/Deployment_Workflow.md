# Deployment Workflow — From Random Allocation to Data-Driven Allocation (Staycity)

## 1) Current operational challenge
Towels and linens (sheets) are currently distributed across floors in an ad-hoc way.
This often produces:
- shortage on floors that need stock,
- surplus on floors that do not,
- urgent requests and repeated porter trips.

The allocation process should be driven primarily by **check-outs**, because check-outs trigger the strongest demand for fresh towels and linen changes.

---

## 2) Phase 0 — Immediate improvement without ML (rule-based baseline)
Before any ML model exists, the property can remove randomness by applying a simple baseline rule:

### Baseline rule concept
- Use `checkouts_floor` as the main signal.
- Convert check-outs into recommended packs for each item type.

Example baseline mapping (conceptual):
- `recommended_towel_packs = checkouts_floor * towel_packs_per_checkout`
- `recommended_linen_packs = checkouts_floor * linen_packs_per_checkout`

The exact multipliers are set by management based on practical experience.

### Why this phase matters
- It creates consistency immediately.
- It produces a measurable process that can later be improved by ML.
- It helps staff trust the approach (“we are no longer guessing”).

---

## 3) Phase 1 — Lightweight data capture (to enable supervised learning)
Currently, the organisation does not record actual demand by floor, and knowledge remains mostly with managers.
To train supervised models, we must capture outcomes.

### Minimal daily log (per floor)
For each floor, record:
- `checkouts_floor`
- `towel_packs_delivered`
- `linen_packs_delivered`
- `urgent_towel_topups` (extra packs requested due to shortage)
- `urgent_linen_topups`

This can be done using a simple spreadsheet or form.
The aim is low overhead: a few minutes per day.

### How this enables labels
After collecting data, define:
- demand labels (`LOW/MEDIUM/HIGH`) per floor and item type,
based on delivered packs + urgent top-ups (i.e., true demand).

---

## 4) Phase 2 — Train a supervised classification model
After collecting 4–8 weeks (or more) of logs, train a classifier to predict:
- towels demand class (LOW/MED/HIGH) per floor for the next day
- linens demand class (LOW/MED/HIGH) per floor for the next day

### Inputs used by the model (examples)
- tomorrow’s `checkouts_floor` (primary)
- `day_of_week`
- seasonality (month)
- recent urgent top-ups
- occupancy / check-ins (if available)

### Why start with classification
- It is easier to operationalise and interpret.
- It aligns with a “par level” approach for stock allocation.
- It can be evaluated with confusion matrix and F1-score for HIGH demand.

---

## 5) Phase 3 — Daily operational use (next-day allocation list)
### Daily run timing
Run the system at a consistent time (e.g., late afternoon) to prepare for the next day.

### Output for staff
Produce a simple allocation plan:
- For each floor:
  - towels: demand class + recommended packs
  - linens: demand class + recommended packs

This output can be shared as:
- a simple dashboard,
- a printed list,
- or a message in the team’s operational channel.

### Human override (important)
The system is decision support:
- managers can override recommendations for special situations
  (e.g., exceptional groups, last-minute changes).

---

## 6) Monitoring, improvement, and maintenance
### What to monitor weekly
- confusion matrix trends (especially under-predicting HIGH demand)
- HIGH-demand recall / F1-score
- operational KPIs:
  - urgent top-up requests,
  - repeated porter trips,
  - delays due to missing stock.

### Handling seasonality and change
Demand patterns shift with:
- events and seasonal peaks,
- policy changes (linen change frequency),
- staffing changes.

The system should be recalibrated or retrained periodically to maintain performance.

---

## 7) Summary
A practical deployment path for Staycity is:
1) remove randomness with a check-out baseline,
2) start simple logging,
3) train a supervised classifier,
4) generate a next-day allocation plan,
5) monitor performance and improve over time.