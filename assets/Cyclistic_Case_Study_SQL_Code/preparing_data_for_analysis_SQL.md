# Preparing Data for Analysis (SQL)
Once the data was thoroughly cleaned, I proceeded to enhance the data table with additional calculated fields to facilitate analysis.
- Added a new column named `ride_length_minutes` to calculate the duration of each ride in minutes using the `DATETIME_DIFF` function.
- Created a `day_of_week` column to extract and store the corresponding weekday from the `started_at` `DATETIME` field using the `FORMAT_DATETIME` function.
- Introduced a month column to identify the month in which each ride began, also utilizing the `FORMAT_DATETIME` function.
- Conducted data mapping verification to ensure all fields were accurately aligned with the original datasets and the defined business requirements.
- The enhanced data was then saved as a new ready to analyze table.

```sql
CREATE TABLE `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready` AS (

SELECT
  ride_id,
  rideable_type,
  started_at,
  ended_at,
  DATETIME_DIFF(ended_at, started_at, MINUTE) AS ride_length_minutes,
  FORMAT_DATETIME("%A", started_at) as day_of_week,
  FORMAT_DATETIME("%B", started_at) as month,
  start_station_name,
  start_station_id,
  end_station_name,
  end_station_id,
  start_lat,
  start_lng,
  end_lat,
  end_lng,
  member_casual

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_enhanced`

)
```
