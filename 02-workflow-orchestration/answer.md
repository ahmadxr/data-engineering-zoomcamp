# Homework 2 Answers (my notes)

---

## Question 1
**Answer:** 128.3 MiB

**My note:** I ran the Kestra flow for Yellow taxi, year 2020, month 12. In the execution logs, I looked at the `extract` task output. It showed the uncompressed file size was about **128.25 MiB**, so I picked **128.3 MiB**.

---

## Question 2
**Answer:** green_tripdata_2020-04.csv

**My note:** This one is just about how Kestra renders variables. The template looks like this:

```
{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv
```

If I plug in `taxi=green`, `year=2020`, `month=04`, I get:

```
green_tripdata_2020-04.csv
```

---

## Question 3
**Answer:** 24,648,499

**My note:** I used DuckDB to count all rows in the 2020 Yellow taxi files. Here's the code I ran:

```python
import duckdb

total = 0
for month in range(1, 13):
    url = f"https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-{month:02d}.parquet"
    count = duckdb.sql(f"SELECT COUNT(*) FROM read_parquet('{url}')").fetchone()[0]
    print(f"Month {month}: {count}")
    total += count

print(f"Total: {total}")
# Output: 24,649,092 → closest answer is 24,648,499
```

---

## Question 4
**Answer:** 1,734,051

**My note:** Same idea as Q3, but for Green taxi data:

```python
import duckdb

total = 0
for month in range(1, 13):
    url = f"https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2020-{month:02d}.parquet"
    count = duckdb.sql(f"SELECT COUNT(*) FROM read_parquet('{url}')").fetchone()[0]
    print(f"Month {month}: {count}")
    total += count

print(f"Total: {total}")
# Output: 1,734,176 → closest answer is 1,734,051
```

---

## Question 5
**Answer:** 1,925,152

**My note:** Just needed to count rows for one file – Yellow taxi, March 2021:

```python
import duckdb

url = "https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2021-03.parquet"
count = duckdb.sql(f"SELECT COUNT(*) FROM read_parquet('{url}')").fetchone()[0]
print(count)
# Output: 1,925,152 ← exact match!
```

---

## Question 6
**Answer:** Add a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration

**My note:** Kestra uses IANA timezone names (like `America/New_York`), not abbreviations like `EST`. Here's what the trigger config looks like:

```yaml
triggers:
  - id: schedule
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 9 * * *"
    timezone: America/New_York
```
