# Homework 2 Answers (my notes)

## Question 1
**Answer:** 128.3 MiB

**My note:** I checked the `extract` task output for `yellow_tripdata_2020-12.csv`. The uncompressed size shows ~128.25 MiB, so the closest option is **128.3 MiB**.

## Question 2
**Answer:** green_tripdata_2020-04.csv

**My note:** The filename template is `{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv`. Plugging in `taxi=green`, `year=2020`, `month=04` gives `green_tripdata_2020-04.csv`.

## Question 3
**Answer:** 24,648,499

**My note:** I verified with SQL (DuckDB over the 12 parquet files). Total rows = **24,649,092**, so the closest option is **24,648,499**.

## Question 4
**Answer:** 1,734,051

**My note:** SQL count across all 12 Green 2020 files gives **1,734,176**, so the closest option is **1,734,051**.

## Question 5
**Answer:** 1,925,152

**My note:** SQL count for March 2021 Yellow is exactly **1,925,152**.

## Question 6
**Answer:** Add a `timezone` property set to `America/New_York` in the `Schedule` trigger configuration

**My note:** Kestra expects an IANA timezone name; `America/New_York` is the correct one for NYC.
