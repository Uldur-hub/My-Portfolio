# BigQuery SQL Cleaning Process

### 1. Merge all data tables into a single new one to be able to efficiently work on all data.
```sql
-- Query to merge all data tables into a single new table

-- To create the new table which will contain all the merged data.
CREATE TABLE `2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned` AS (

  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202401-divvy-tripdata-uncleaned`
-- UNION ALL to merge all data including duplicates between data tables. Will check for duplicates later in the cleaning process.
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202402-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202403-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202404-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202405-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202406-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202407-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202408-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202409-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202410-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202411-divvy-tripdata-uncleaned`
  UNION ALL
  SELECT * FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.202412-divvy-tripdata-uncleaned`

)
```

### 2. Standardize the formatting.
By applying the `UPPER`, `LOWER`, or `INITCAP (PROPER)` functions as appropriate for each column. Remove any leading or trailing whitespace using the `TRIM` function, `CAST` the data to its appropriate type and save the formatted data into a new table.

```sql
-- To create the new table which will contain all the formatted data.
CREATE TABLE `2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_formatted` AS (

SELECT
  CAST(TRIM(UPPER(ride_id), ' ') AS STRING) AS ride_id, -- UPPER to make all characters upper case.
  CAST(TRIM(LOWER(rideable_type), ' ') AS STRING) AS rideable_type, -- LOWER to make all characters lower case.
  CAST(started_at AS DATETIME) AS started_at, -- DATETIME to ensure format.
  CAST(ended_at AS DATETIME) AS ended_at,
  CAST(TRIM(INITCAP(start_station_name), ' ') AS STRING) AS start_station_name, -- INITCAP to make every first character upper case.
  CAST(TRIM(UPPER(start_station_id), ' ') AS STRING) AS start_station_id, -- TRIM to remove any leading or trailing whitespaces.
  CAST(TRIM(INITCAP(end_station_name), ' ') AS STRING) AS end_station_name, -- CAST to convert into desired data type.
  CAST(TRIM(UPPER(end_station_id), ' ') AS STRING) AS end_station_id,
  CAST(start_lat AS FLOAT64) AS start_lat,
  CAST(start_lng AS FLOAT64) AS start_lng,
  CAST(end_lat AS FLOAT64) AS end_lat,
  CAST(end_lng AS FLOAT64) AS end_lng,
  CAST(TRIM(LOWER(member_casual), ' ') AS STRING) AS member_casual


FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`

)

```

### 3. Check for duplicates based on the primary key.
No duplicate values were found.

```sql
-- Check for duplicates based on the primary key.
SELECT
  DISTINCT COUNT(ride_id) AS unique_ride_ids, -- To count how many unique IDs we have.
  COUNT(ride_id) AS ride_id_count -- To count how many overall IDs we have.

FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`

-- Since the unique IDs and the overall IDs match that means there's no duplicate values.
```

### 4. DELETE NULL values across all fields.
1,652,259 NULL values were found and removed.

```sql
-- DELETE NULL values across all fields.
DELETE

FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_formatted`

WHERE
  ride_id IS NULL OR
  rideable_type IS NULL OR
  started_at IS NULL OR
  ended_at IS NULL OR
  start_station_name IS NULL OR
  start_station_id IS NULL OR
  end_station_name IS NULL OR
  end_station_id IS NULL OR
  start_lat IS NULL OR
  start_lng IS NULL OR
  end_lat IS NULL OR
  end_lng IS NULL OR
  member_casual IS NULL
```

### 5. Verify consistency of primary key.
Used `DISTINCT` function to measure the `LENGTH()` of each primary key (ride_id). All IDs have a length of 16 therefore the primary key is consistent.

```sql
-- Verified the consistency of the primary key (ride_id) using the DISTINCT function to measure the LENGTH() of each ID.
SELECT
  DISTINCT LENGTH(Ride_id) as ride_id_length


FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_formatted`

-- All IDs have a length of 16 therefore the primary key is consistent.
```

### 6. Verify consistency of the rest of the fields.
`DISTINCT` to search for inconsistencies.

```sql
-- To verify the consistency of each field.
SELECT
  DISTINCT rideable_type

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`
```

In the `rideable_type` field, I noticed some records labeled as `electric_scooter`, so I decided to investigate further by counting how many instances of `electric_scooter` were present.
```sql
-- To count how many electric_scooter records we have in the rideable_type field.
SELECT
  COUNT(*)

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`

WHERE
  rideable_type = `electric_scooter`
```
A total of 47,827 `electric_scooter` records were found, so I investigated further by finding the timeframe of these records.
```sql
-- Find the electric_scooter data timeframe.
SELECT
  MIN(started_at) AS from_date,
  MAX(ended_at) AS to_date

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`

WHERE
  rideable_type = `electric_scooter`
```
Data for `electric_scooters` was only recorded during the month of September. Given that this analysis focuses specifically on bikes and the timeframe for scooter data is inconsistent, I have decided to exclude it from the analysis.

```sql
DELETE

FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`

WHERE
  rideable_type = 'electric_scooter'
```
Moving forward. Test data was found in `end_station_id` and therefore removed.
```sql
SELECT
  DISTINCT end_station_id

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_uncleaned`
```
Verified consistency for the latitude and longitude fields to check they’re within the correct parameters.
```sql
SELECT
  MIN(start_lat) AS start_lat_min,
  MAX(start_lat) AS start_lat_max,
  MIN(start_lng) AS start_lng_min,
  MAX(start_lng) AS start_lng_max,
  MIN(end_lat) AS end_lat_min,
  MAX(end_lat) AS end_lat_mac,
  MIN(end_lng) AS end_lng_min,
  MAX(end_lng) AS end_lng_max

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_formatted`
```


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
