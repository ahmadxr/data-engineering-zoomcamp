# Module 3 Homework — Notes on How I Got the Answers

This file documents the steps I used to get each answer. I kept it short and focused on what I actually ran or checked.

## Q1 — Count of records (Jan–Jun 2024)

I downloaded the six parquet files (2024-01 through 2024-06) and counted the rows locally with DuckDB:

- Files: yellow_tripdata_2024-01.parquet … yellow_tripdata_2024-06.parquet
- Query (DuckDB): SELECT COUNT(*) FROM read_parquet('.../yellow_tripdata_2024-*.parquet')
- Result: 20,332,093

## Q2 — Estimated bytes read (distinct PULocationID)

I ran the same query against both the external table and the materialized table in BigQuery and checked the “This query will process …” estimate before running:

- Query: SELECT COUNT(DISTINCT PULocationID) FROM <table>
- Estimate: 18.82 MB (external) and 47.60 MB (materialized)

## Q3 — Columnar storage explanation

I compared the estimates for:

- SELECT PULocationID FROM <materialized_table>
- SELECT PULocationID, DOLocationID FROM <materialized_table>

The second query reads more bytes because BigQuery is columnar and only scans the columns you request. Two columns means more data scanned than one.

## Q4 — Fare amount = 0

I ran:

- Query: SELECT COUNT(*) FROM <materialized_table> WHERE fare_amount = 0
- Result: 546,578

## Q5 — Best partition + cluster strategy

Given the filter is always on tpep_dropoff_datetime and ordering by VendorID, the best strategy is:

- Partition by tpep_dropoff_datetime
- Cluster by VendorID

## Q6 — Partition benefits

I ran the same query on the non-partitioned and partitioned tables and compared estimated bytes:

- Query: SELECT DISTINCT VendorID FROM <table> WHERE tpep_dropoff_datetime BETWEEN '2024-03-01' AND '2024-03-15'
- Estimate: 310.24 MB (non-partitioned), 26.84 MB (partitioned)

## Q7 — External table storage

External table data remains in the GCS bucket, so the answer is GCP Bucket.

## Q8 — Clustering best practices

Clustering is not always best practice for every table, so the correct answer is False.

## Q9 — Table scan with COUNT(*)

SELECT COUNT(*) on the materialized table scans the full table (all rows), so the estimate equals the full table size. The exact byte estimate depends on the table size shown in BigQuery for that dataset.
