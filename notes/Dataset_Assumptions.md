# Dataset Assumptions — Towels & Linens Demand by Floor

## Prediction unit (one row)
- `date`
- `floor`
- `item_type` (e.g., `towels`, `linens`)

## Inputs (features)
Primary driver (expected):
- `checkouts_floor_next_day` (or same-day check-outs for next-day planning depending on workflow)

Optional/secondary drivers (if available):
- `occupied_rooms_floor`
- `checkins_floor_next_day`
- `avg_stay_length_floor` (proxy for long-stay vs short-stay)
- `day_of_week`
- `month/season`
- `special_event_flag` (city-wide events can affect occupancy and turnover)
- `recent_extra_requests_floor_item` (count of urgent top-ups requested)

## Output (label / target)
We propose a **classification** label per (date, floor, item_type):
- `LOW`, `MEDIUM`, `HIGH` demand

### How to create labels
Since there is currently no formal tracking, labels can be created after introducing simple recording:
- Measure actual packs used or requested for each floor/item daily.
- Define thresholds with managers (example approach):
  - LOW: below X packs
  - MEDIUM: X–Y packs
  - HIGH: above Y packs
Alternatively, define labels using percentiles (terciles) after collecting enough history.

## Data capture plan (practical)
Phase 1 (immediate):
- Introduce a simple daily log (spreadsheet or form) per floor:
  - check-outs
  - towels packs delivered
  - linens packs delivered
  - urgent top-up requests (yes/no + count)

Phase 2 (after 4–8 weeks):
- Use the recorded history as training data for a supervised classifier.