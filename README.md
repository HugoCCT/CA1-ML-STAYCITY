# Machine Learning Tools CA1 — Staycity Dublin
## Topic: Daily towel & linen demand allocation by floor

### Goal
Design and evaluate a Machine Learning (ML) approach to improve daily allocation of towels and linens (sheets) across floors in a Staycity property in Dublin. The current process is largely ad-hoc, which often causes shortages on floors that need stock and surplus on floors that do not.

### Business problem (high level)
Housekeeping and porters need the right quantity of towels and linens on each floor each day. Allocation is currently not driven by check-outs (the main demand driver), creating operational delays and rework.

### Proposed ML direction
- Primary approach: **Supervised learning + Classification** (predict `LOW / MEDIUM / HIGH` demand per floor for the next day).
- Alternative approach: **Unsupervised learning + Clustering** (discover “types of days/floors” but less direct for decision-making).

### Repository structure
- `docs/` — report draft/final, cover sheet, references
- `notes/` — working notes (outline, dataset assumptions, metrics, deployment workflow)
- `assets/` — diagrams/screenshots if needed

### Status
In progress — notes and report draft are being developed with incremental commits.