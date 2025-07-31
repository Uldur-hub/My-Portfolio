# BigQuery SQL Cleaning Process

1. First step will be merging all data tables into a single new one to be able to efficiently work on all data.
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

