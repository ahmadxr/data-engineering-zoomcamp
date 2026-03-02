# Module 5 Homework Answers (Bruin)

## Q1. Bruin Pipeline Structure
**Answer:** `.bruin.yml` and `pipeline/` with `pipeline.yml` and `assets/`.

**Short explanation:** Bruin requires project-level config in `.bruin.yml`, and each pipeline is defined by `pipeline.yml` with an adjacent `assets/` folder. The pipeline can be inside `pipeline/` (common layout) or flat at root.

---

## Q2. Materialization Strategies
**Answer:** `time_interval`.

**Short explanation:** `time_interval` is designed for incremental processing by time windows (e.g., monthly by `pickup_datetime`) and supports replacing data for a selected interval.

---

## Q3. Pipeline Variables
**Answer:** `bruin run --var 'taxi_types=["yellow"]'`

**Short explanation:** `--var` is the correct flag for overriding `pipeline.yml` variables. Since `taxi_types` is an array, the override must be passed as an array JSON value.

---

## Q4. Running with Dependencies
**Answer:** `bruin run ingestion/trips.py --downstream`

**Short explanation:** `--downstream` runs the selected asset and everything that depends on it, which matches “run it plus all downstream assets.”

---

## Q5. Quality Checks
**Answer:** `name: not_null`

**Short explanation:** The built-in `not_null` check directly enforces that `pickup_datetime` cannot be NULL.

---

## Q6. Lineage and Dependencies
**Answer:** `bruin lineage`

**Short explanation:** Bruin’s lineage command shows upstream/downstream asset dependencies and is the command intended for dependency graph/lineage inspection.

---

## Q7. First-Time Run
**Answer:** `--full-refresh`

**Short explanation:** `--full-refresh` rebuilds tables from scratch (truncate/recreate behavior where applicable), which is the intended flag for clean initial rebuilds.
