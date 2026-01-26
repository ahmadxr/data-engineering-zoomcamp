# Homework 1 – Simple Notes

Short, easy notes for each question. I kept this beginner‑friendly and straight to the point.

## Question 1 – Docker image check
- Open the `python:3.13` image in bash.
- Run `pip --version` to see the pip version.

## Question 2 – Docker Compose networking
- Use the service name and container port: `db:5432`

## Data download
- Download the green taxi parquet file for 2025‑11.
- Download the taxi zone lookup CSV.

## Question 3 – Counting short trips
- Load the parquet file.
- Keep trips in November 2025.
- Count trips with `trip_distance` ≤ 1 mile.

## Question 4 – Longest trip by pickup day
- Filter to November 2025.
- Ignore trips ≥ 100 miles.
- Find which pickup day has the biggest trip distance.

## Question 5 – Biggest pickup zone on 2025-11-18
- Filter trips to November 18.
- Join with the zone lookup.
- Sum `total_amount` by pickup zone and take the top one.

## Question 6 – Largest tip from East Harlem North pickups
- Filter to November 2025.
- Keep pickups from “East Harlem North”.
- Find the dropoff zone with the largest tip.

## Question 7 – Terraform workflow
No commands run here. The correct sequence is:

1. `terraform init`
2. `terraform apply -auto-approve`
3. `terraform destroy`
