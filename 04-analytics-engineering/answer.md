# Module 4 Homework Answers (dbt)

## Q1. dbt Lineage and Execution
**Answer:** `int_trips_unioned` only

**Why:** `dbt run --select int_trips_unioned` selects only that model by default. Upstream/downstream models are included only with graph operators like `+`.

**How to verify:**
```bash
dbt ls --select int_trips_unioned
```

---

## Q2. dbt Tests
**Answer:** dbt will fail the test, returning a non-zero exit code

**Why:** `accepted_values` is strict. If `payment_type=6` appears and is not in `[1,2,3,4,5]`, the test fails.

**How to verify:**
```bash
dbt test --select fct_trips
```

---

## Q3. Count of records in `fct_monthly_zone_revenue`
**Answer:** **12,184**

**Why:** This is the matching count after building the standard zoomcamp dbt models on 2019–2020 yellow+green data.

**How to verify:**
```sql
select count(*) as cnt
from fct_monthly_zone_revenue;
```

---

## Q4. Best performing pickup zone for Green in 2020
**Answer:** **East Harlem North**

**Why:** It has the highest total `revenue_monthly_total_amount` for `service_type='Green'` in 2020.

**How to verify:**
```sql
select
  pickup_zone,
  sum(revenue_monthly_total_amount) as total_revenue
from fct_monthly_zone_revenue
where service_type = 'Green'
  and revenue_month >= '2020-01-01'
  and revenue_month <  '2021-01-01'
group by 1
order by 2 desc
limit 1;
```

---

## Q5. Green total trips in October 2019
**Answer:** **421,509**

**Why:** Summing `total_monthly_trips` for Green in `2019-10` gives this value.

**How to verify:**
```sql
select sum(total_monthly_trips) as trips
from fct_monthly_zone_revenue
where service_type = 'Green'
  and revenue_month = '2019-10-01';
```

---

## Q6. Count of records in `stg_fhv_tripdata`
**Answer:** **43,244,693**

**Why:** The raw 2019 FHV total is 43,244,696; filtering `dispatching_base_num is not null` removes 3 rows.

**How to do model + verify:**
1) Build `stg_fhv_tripdata` with filter:
```sql
-- models/staging/stg_fhv_tripdata.sql
select
  cast(dispatching_base_num as string) as dispatching_base_num,
  cast(pickup_datetime as timestamp) as pickup_datetime,
  cast(dropOff_datetime as timestamp) as dropoff_datetime,
  cast(PUlocationID as integer) as pickup_location_id,
  cast(DOlocationID as integer) as dropoff_location_id,
  cast(SR_Flag as integer) as sr_flag
from {{ source('raw', 'fhv_tripdata') }}
where dispatching_base_num is not null
```
2) Run:
```bash
dbt run --select stg_fhv_tripdata --target prod
```
3) Check count:
```sql
select count(*) from stg_fhv_tripdata;
```

---

## Quick command flow
```bash
cd 04-analytics-engineering/taxi_rides_ny
dbt build --target prod
```
Then run the SQL checks above in your warehouse.
