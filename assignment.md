# Assignment

## Brief

Write the SQL statements for the following questions.

## Instructions

Paste the answer as SQL in the answer code section below each question.

### Question 1

Let's revisit our `london_bicycle` dbt project. Modify the `dim_station.sql` model to include the following columns:

- `total_duration` (sum of `duration` for each station in seconds)
- `total_starts` (count of `start_station_name` for each station)
- `total_ends` (count of `end_station_name` for each station)

Then, rebuild the models with the following command to see if the changes are correct:

```bash
dbt run
```

Answer:

Paste the `dim_station.sql` model here:

```sql
with station_data as (
    select
        station_name,
        sum(duration) as total_duration,
        count(case when station_name = start_station_name then 1 end) as total_starts,
        count(case when station_name = end_station_name then 1 end) as total_ends
    from {{ ref('fact_rides') }}
    cross join (
        select distinct start_station_name as station_name from {{ ref('fact_rides') }}
        union
        select distinct end_station_name as station_name from {{ ref('fact_rides') }}
    ) as stations
    where station_name in (start_station_name, end_station_name)
    group by station_name
)

select
    station_name,
    total_duration,
    total_starts,
    total_ends
from station_data

dbt run 

| station_name | total_duration | total_starts | total_ends | 
| "Central Park" | 12,450 | 320 | 298 | 
| "Downtown Hub" | 8,900 | 210 | 215 | 
| "East Station" | 5,300 | 150 | 160 | 
| ... | ... | ... | ... | 


⚠️ Note: These values are illustrative. Your actual results will depend on the data in fact_rides.




```

## Submission

- Submit the URL of the GitHub Repository that contains your work to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
