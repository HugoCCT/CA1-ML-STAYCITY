# Deployment Workflow — From Baseline to ML

## Current issue
Allocation of towels and linens across floors is ad-hoc. The process should be driven by check-outs, but this is not consistently communicated or tracked.

## Phase 0: No-ML baseline (immediate improvement)
Implement a simple rule-based allocation:
- Use `checkouts_floor` as the main signal.
- Convert check-outs into recommended stock levels:
  - Example: check-outs -> number of towel/linen packs
- This already reduces randomness and creates consistency.

## Phase 1: Data capture (enable supervised learning)
Introduce a daily log:
- Per floor: check-outs, packs delivered (towels/linens), urgent top-ups
- Keep it lightweight so staff can realistically maintain it.

## Phase 2: Supervised classification model
Train a classifier to predict `LOW/MEDIUM/HIGH` demand per floor/item for the next day using:
- check-outs (primary)
- occupancy, day-of-week, seasonality, and recent urgent requests (optional)

## Phase 3: Operational usage
Daily run (e.g., late afternoon):
- Produce a “next-day allocation list”:
  - Floor -> towels demand class -> recommended packs
  - Floor -> linens demand class -> recommended packs
- Provide the output to porters/housekeeping via a simple dashboard or printed list.

## Monitoring & continuous improvement
- Track errors and shortages weekly.
- Update thresholds/labels if the process changes (seasonality, policy changes, staffing changes).