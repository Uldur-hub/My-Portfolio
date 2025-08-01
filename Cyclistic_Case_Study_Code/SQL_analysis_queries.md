# SQL Analysis Queries
In the next phase of the analysis, I will examine the data using calculations and descriptive methods to identify trends and relationships. The goal is to generate insights that will support answering our business question “How do annual members and casual riders use Cyclistic bikes differently?” and addressing key objectives.

## Table of Contents

- [1. Most Popular User Type](#most-popular-user-type)
- [2. Most popular type of bike overall](#most-popular-type-of-bike-overall)
- [3. Most popular type of bike per type of user](#most-popular-type-of-bike-per-type-of-user)
- [4. Total rides per month by type of user](#total-rides-per-month-by-type-of-user)
- [5. Percentage of rides per month by type of user](#percentage-of-rides-per-month-by-type-of-user)
- [6. Total rides per weekday by type of user](#total-rides-per-weekday-by-type-of-user)
- [7. Percentage of rides per weekday by type of user](#percentage-of-rides-per-weekday-by-type-of-user)
- [8. Average ride length by type of user](#average-ride-length-by-type-of-user)
- [9. Average ride length by weekday by type of user](#average-ride-lenght-by-weekday-by-type-of-user)
- [10. Total rides by hour by type of user](#total-rides-by-hour-by-type-of-user)
- [11. Most common & least common start station overall](#most-common-and-least-common-start-station-overall)
- [12. Most common & least common start stations by type of user](#most-common-and-least-common-start-stations-by-type-of-user)
- [13. Most common & least common end station overall](#most-common-and-least-common-end-station-overall)
- [14. Most common & least common end station by type of user](#most-common-and-least-common-end-station-by-type-of-user)
- [15. Most common & least common routes overall](#most-common-and-least-common-routes-overall)
- [16. Most common & least common routes by type of user](#most-common-and-least-common-routes-by-type-of-user)

## Queries

### Most Popular User Type

The most popular type of user is "Member" type covering 64% of the population.
```sql
-- Query to find the most popular user type with count and percentage.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
  -- Subquery to count all the member users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "member")) AS member_count,

  -- Subquery to calculate the percentage of member user count vs total.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "member") * 100 / COUNT(*)) AS member_percent,

  -- Subquery to count all the casual users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "casual")) AS casual_count,

  -- Subquery to calculate the percentage of casual user count vs total.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "casual") * 100 / COUNT(*)) AS casual_percent,

  -- Subquery to sum member_percent and casual_percent to make sure it adds up to a 100% and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "member") * 100 / COUNT(*)) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "casual") * 100 / COUNT(*)) AS total_percentage,

  -- Query to count total_users.
  COUNT(*) AS total_users,
  
  -- Subquery to sum member_count and casual_count to make sure it matches with the total_users and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "member")) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE member_casual = "casual")) AS member_casual_sum


FROM clean_cyclistic_dataset
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/most_pop_user_type.png)

**[Back to Top](#table-of-contents)**

### Most popular type of bike overall
`classic_bike` with over 65.5% of preference among users.
```sql
-- Query to find the most popular bike type with count and percentage.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
  -- Subquery to count all the classic_bikes.
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike") AS classic_bike_count,

  -- Subquery to calculate the percentage of classic_bikes count vs total.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike") * 100 / COUNT(*)) AS classic_bike_percent,

  -- Subquery to count all the electric_bikes.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike")) AS electric_bike_count,

  -- Subquery to calculate the percentage of electric_bikes count vs total.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike") * 100 / COUNT(*)) AS electric_bike_percent,

  -- Subquery to sum classic_bike_percent and electric_bike_percent to make sure it adds up to a 100% and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike") * 100 / COUNT(*)) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike") * 100 / COUNT(*)) AS total_percentage,

  -- Query to count total_rides.
  COUNT(*) AS total_rides,
  
  -- Subquery to sum classic_bike_count and electric_bike_count to make sure it matches with the total_rides and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike")) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike")) AS classic_electric_sum


FROM clean_cyclistic_dataset
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/most_pop_bike_overall.png)

**[Back to Top](#table-of-contents)**

### Most popular type of bike per type of user
- Member Users: Classic Bike with 66%.
```sql
-- Query to find the most popular bike type between member users with count and percentage.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
  -- 1. Subquery to count all the classic_bikes used by member users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "member")) AS mem_classic_bike_count,

  -- 2. Subquery to calculate the percentage of classic_bikes usage by member users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "member") * 100 / COUNT(*)) AS mem_classic_bike_percent,

  -- 3. Subquery to count all the electric_bikes used by member users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "member")) AS mem_electric_bike_count,

  -- 4. Subquery to calculate the percentage of electric_bikes usage by member users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "member") * 100 / COUNT(*)) AS mem_electric_bike_percent,

  -- 5. Subquery to sum mem_classic_bike_percent and mem_electric_bike_percent to make sure it adds up to a 100% and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "member") * 100 / COUNT(*)) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "member") * 100 / COUNT(*)) AS mem_total_percentage,

  -- 6. Query to count total member rides.
  COUNT(*) AS total_member_rides,

  -- 7. Subquery to sum mem_classic_bike_count and mem_electric_bike_count to make sure it matches with the total_member_rides and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "member")) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "member")) AS mem_classic_electric_sum

FROM clean_cyclistic_dataset

WHERE member_casual = "member"
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/members_most_pop_bike.png)

- Casual Users: Classic Bike with 64.7%
```sql
-- Query to find the most popular bike type between casual users with count and percentage.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
  -- 1. Subquery to count all the classic_bikes used by casual users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "casual")) AS cas_classic_bike_count,

  -- 2. Subquery to calculate the percentage of classic_bikes usage by casual users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "casual") * 100 / COUNT(*)) AS cas_classic_bike_percent,

  -- 3. Subquery to count all the electric_bikes used by casual users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "casual")) AS cas_electric_bike_count,

  -- 4. Subquery to calculate the percentage of electric_bikes usage by casual users.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "casual") * 100 / COUNT(*)) AS cas_electric_bike_percent,

  -- 5. Subquery to sum cas_classic_bike_percent and cas_electric_bike_percent to make sure it adds up to a 100% and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "casual") * 100 / COUNT(*)) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "casual") * 100 / COUNT(*)) AS cas_total_percentage,

  -- 6. Query to count total casual rides.
  COUNT(*) AS total_casual_rides,

  -- 7. Subquery to sum cas_classic_bike_count and cas_electric_bike_count to make sure it matches with the total_casual_rides and verify accuracy of calculations.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "classic_bike" AND member_casual = "casual")) + 
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE rideable_type = "electric_bike" AND member_casual = "casual")) AS cas_classic_electric_sum

FROM clean_cyclistic_dataset

WHERE member_casual = "casual"
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_most_pop_bike.png)

**[Back to Top](#table-of-contents)**

### Total rides per month by type of user
- Member Users:
```sql
-- Query to find the total rides per month by member users.

SELECT
  month,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  month

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_total_rides_month.png)

- Casual Users:
```sql
-- Query to find the total rides per month by casual users.

SELECT
  month,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  month

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_total_rides_month.png)

**[Back to Top](#table-of-contents)**

### Percentage of rides per month by type of user
- Member Users:
```sql
-- Query to find the percent of member user rides per month.

--Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
-- Subquery to calculate the percent of member user rides rides on January.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "January" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "January")) AS jan_member_percent,

-- Subquery to calculate the percent of member user rides rides on February.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "February" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "February")) AS feb_member_percent,

-- Subquery to calculate the percent of member user rides rides on March.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "March" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "March")) AS march_member_percent,

-- Subquery to calculate the percent of member user rides rides on April.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "April" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "April")) AS apr_member_percent,

-- Subquery to calculate the percent of member user rides rides on May.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "May" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "May")) AS may_member_percent,

-- Subquery to calculate the percent of member user rides rides on June.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "June" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "June")) AS june_member_percent,

-- Subquery to calculate the percent of member user rides rides on July.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "July" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "July")) AS july_member_percent

-- Subquery to calculate the percent of member user rides rides on August.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "August" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "August")) AS aug_member_percent

-- Subquery to calculate the percent of member user rides rides on September.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "September" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "September")) AS sept_member_percent

-- Subquery to calculate the percent of member user rides rides on October.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "October" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "October")) AS oct_member_percent

-- Subquery to calculate the percent of member user rides rides on November.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "November" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "November")) AS nov_member_percent

-- Subquery to calculate the percent of member user rides rides on December.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "December" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "December")) AS dec_member_percent

--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_percent_rides_month.png)
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_percent_rides_month_2.png)

- Casual Users:
```sql
-- Query to find the percent of casual user rides per month.

--Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
-- Subquery to calculate the percent of casual user rides rides on January.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "January" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "January")) AS jan_member_percent,

-- Subquery to calculate the percent of casual user rides rides on February.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "February" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "February")) AS feb_member_percent,

-- Subquery to calculate the percent of casual user rides rides on March.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "March" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "March")) AS march_member_percent,

-- Subquery to calculate the percent of casual user rides rides on April.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "April" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "April")) AS apr_member_percent,

-- Subquery to calculate the percent of casual user rides rides on May.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "May" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "May")) AS may_member_percent,

-- Subquery to calculate the percent of casual user rides rides on June.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "June" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "June")) AS june_member_percent,

-- Subquery to calculate the percent of casual user rides rides on July.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "July" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "July")) AS july_member_percent

-- Subquery to calculate the percent of casual user rides rides on August.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "August" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "August")) AS aug_member_percent

-- Subquery to calculate the percent of casual user rides rides on September.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "September" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "September")) AS sept_member_percent

-- Subquery to calculate the percent of casual user rides rides on October.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "October" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "October")) AS oct_member_percent

-- Subquery to calculate the percent of casual user rides rides on November.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "November" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "November")) AS nov_member_percent

-- Subquery to calculate the percent of casual user rides rides on December.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "December" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE month = "December")) AS dec_member_percent

--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_percent_rides_month.png)
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_percent_rides_month_2.png)

**[Back to Top](#table-of-contents)**

### Total rides per weekday by type of user
- Member Users:
```sql
-- Query to find the total rides per weekday by member users.

SELECT
  day_of_week,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  day_of_week

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_week_rides.png)

- Casual Users:
```sql
-- Query to find the total rides per weekday by casual users.

SELECT
  day_of_week,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  day_of_week

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_week_rides.png)

**[Back to Top](#table-of-contents)**

### Percentage of rides per weekday by type of user
- Member Users:
```sql
-- Query to find the percent of member user rides per weekday.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
-- Subquery to calculate the percent of member user rides rides on Monday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Monday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Monday")) AS mon_member_percent,

-- Subquery to calculate the percent of member user rides rides on Tuesday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Tuesday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Tuesday")) AS tues_member_percent,

-- Subquery to calculate the percent of member user rides rides on Wednesday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Wednesday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Wednesday")) AS wed_member_percent,

-- Subquery to calculate the percent of member user rides rides on Thursday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Thursday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Thursday")) AS thurs_member_percent,

-- Subquery to calculate the percent of member user rides rides on Friday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Friday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Friday")) AS frid_member_percent,

-- Subquery to calculate the percent of member user rides rides on Saturday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Saturday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Saturday")) AS sat_member_percent,

-- Subquery to calculate the percent of member user rides rides on Sunday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Sunday" AND member_casual = "member") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Sunday")) AS sun_member_percent

--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_percent_week_rides.png)

- Casual Users:
```sql
-- Query to find the percent of casual user rides per weekday.

-- Define a name for the table to make code cleaner.
WITH clean_cyclistic_dataset AS (
  SELECT *
  FROM `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`)

SELECT
-- Subquery to calculate the percent of casual user rides rides on Monday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Monday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Monday")) AS mon_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Tuesday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Tuesday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Tuesday")) AS tues_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Wednesday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Wednesday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Wednesday")) AS wed_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Thursday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Thursday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Thursday")) AS thurs_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Friday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Friday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Friday")) AS frid_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Saturday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Saturday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Saturday")) AS sat_casual_percent,

-- Subquery to calculate the percent of casual user rides rides on Sunday.
  ((SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Sunday" AND member_casual = "casual") * 100 / 
  (SELECT COUNT(*) FROM clean_cyclistic_dataset WHERE day_of_week = "Sunday")) AS sun_casual_percent

--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_percent_week_rides.png)

**[Back to Top](#table-of-contents)**

### Average ride length by type of user
```sql
-- Query to find Avg ride time by type of user.

SELECT
  member_casual,
  AVG(ride_length_minutes) AS avg_trip_time

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  member_casual

--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/avg_ride_length_by_user.png)

**[Back to Top](#table-of-contents)**

### Average ride length by weekday by type of user
- Member Users:
```sql
-- Query to find Avg ride time by weekday by member users.

SELECT
  day_of_week,
  AVG(ride_length_minutes) AS avg_trip_time

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  day_of_week

ORDER BY
  avg_trip_time DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_avg_ride_length_weekday.png)

- Casual Users:
```sql
-- Query to find Avg ride time by weekday by casual users.

SELECT
  day_of_week,
  AVG(ride_length_minutes) AS avg_trip_time

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  day_of_week

ORDER BY
  avg_trip_time DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_avg_ride_length_weekday.png)

**[Back to Top](#table-of-contents)**

### Total rides by hour by type of user
- Member Users:
```sql
-- Query to find the total rides per hour by member users.

SELECT
  EXTRACT(HOUR FROM started_at) AS hour,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  hour

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_hour_rides.png)

- Casual Users:
```sql
-- Query to find the total rides per hour by casual users.

SELECT
  EXTRACT(HOUR FROM started_at) AS hour,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  hour

ORDER BY
  total_rides DESC
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_hour_rides.png)

**[Back to Top](#table-of-contents)**

### Most common and least common start station overall
- Most Common Start Station Overall:
```sql
-- Query to find the most common start stations overall.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  start_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/most_pop_start_stat_overall.png)

- Least Common Start Station Overall:
```sql
-- Query to find the least common start stations overall.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  start_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/least_pop_start_stat_overall.png)

**[Back to Top](#table-of-contents)**

### Most common and least common start stations by type of user
- Member Most Common Start Stations:
```sql
-- Query to find the most common start stations by member users.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  start_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_most_pop_start_stat.png)

- Member Least Common Start Stations:
```sql
-- Query to find the least common start stations by member users.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  start_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_least_pop_start_stat.png)

- Casual Most Common Start Stations:
```sql
-- Query to find the most common start stations by casual users.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  start_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_most_pop_start_stat.png)

- Casual Least Common Start Stations:
```sql
-- Query to find the least common start stations by casual users.

SELECT
  start_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  start_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_least_pop_start_stat.png)

**[Back to Top](#table-of-contents)**

### Most common and least common end station overall
- Most Common End Station Overall:
```sql
-- Query to find the most common end stations overall.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/most_pop_end_stat_overall.png)

- Least Common End Stations Overall:
```sql
-- Query to find the least common end stations overall.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/least_pop_end_stat_overall.png)

**[Back to Top](#table-of-contents)**

### Most common and least common end station by type of user
- Member Most Common End Stations:
```sql
-- Query to find the most common end stations by member users.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_most_pop_end_stat.png)

- Member Least Common End Stations:
```sql
-- Query to find the least common end stations by member users.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_least_pop_end_stat.png)

- Casual Most Common End Stations:
```sql
-- Query to find the most common end stations by Casual users.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "Casual"

GROUP BY
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_most_pop_end_stat.png)

- Casual Least Common End Stations:
```sql
-- Query to find the least common end stations by Casual users.

SELECT
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "Casual"

GROUP BY
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_least_pop_end_stat.png)

**[Back to Top](#table-of-contents)**

### Most common and least common routes overall
- Most Common Routes Overall:
```sql
-- Query to find the most common routes overall.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/most_pop_route_overall.png)

- Least Common Routes Overall:
```sql
-- Query to find the least common routes overall.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/least_pop_routes_overall.png)

**[Back to Top](#table-of-contents)**

### Most common and least common routes by type of user
- Member Most Common Routes:
```sql
-- Query to find the most common routes by member users.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_most_pop_routes.png)

- Member Least Common Routes:
```sql
-- Query to find the least common routes by member users.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "member"

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/memb_least_pop_routes.png)

- Casual Most Common Routes:
```sql
-- Query to find the most common routes by Casual users.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "Casual"

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides DESC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_most_pop_routes.png)

- Casual Least Common Routes:
```sql
-- Query to find the least common routes by casual users.

SELECT
  start_station_name,
  end_station_name,
  COUNT(*) AS total_rides

FROM
  `cyclistic-case-study-457819.2024_divvy_tripdata_uncleaned.2024_cyclistic_tripdata_ready`

WHERE
  member_casual = "casual"

GROUP BY
  start_station_name,
  end_station_name

ORDER BY
  total_rides ASC

LIMIT 10
--
```
![Banner](https://raw.githubusercontent.com/Uldur-hub/My-Portfolio/main/assets/Cyclistic_SQL_Queries/casual_least_pop_routes.png)

If you actually made it all the way down here, I appreciate you :)
