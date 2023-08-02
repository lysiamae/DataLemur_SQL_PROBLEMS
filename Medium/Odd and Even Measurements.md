# Odd and Even Measurements
#### [Medium]
  ---
Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns. Refer to the Example Output below for the desired format.

Definition:

- Within a day, measurements taken at 1st, 3rd, and 5th times are considered odd-numbered measurements, and measurements taken at 2nd, 4th, and 6th times are considered even-numbered measurements.

#### Table: measurements 
|Column Name|	Type|
| ---- | ----|
|measurement_id	|integer|
|measurement_value|	decimal|
|measurement_time|	datetime|


#### measurements Example Input:
|measurement_id|	measurement_value|	measurement_time|
| ----- | ----- | ----|
|131233	|1109.51|	07/10/2022 09:00:00|
|135211	|1662.74|	07/10/2022 11:00:00|
|523542	|1246.24|	07/10/2022 13:15:00|
|143562	|1124.50|	07/11/2022 15:00:00|
|346462|	1234.14| 07/11/2022 16:45:00|


#### Example Output:
|measurement_day	|odd_sum|	even_sum|
| ----- | ---- | ----|
|07/10/2022 00:00:00|	2355.75|	1662.74|
|07/11/2022 00:00:00|	1124.50|	1234.14|


##### Explanation
Explanation
Based on the results,
- On 07/10/2022, the sum of the odd-numbered measurements is 2355.75, while the sum of the even-numbered measurements is 1662.74.
- On 07/11/2022, there are only two measurements available. The sum of the odd-numbered measurements is 1124.50, and the sum of the even-numbered measurements is 1234.14.

The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH CTE AS (SELECT measurement_id, measurement_value, measurement_time, 
ROW_NUMBER() OVER(PARTITION BY EXTRACT(day FROM measurement_time) ORDER BY EXTRACT(hour FROM measurement_time)) as rn
FROM measurements)


SELECT CAST(measurement_time AS DATE) as measurement_day, 
SUM(CASE WHEN rn % 2 != 0 THEN measurement_value else 0 end) as odd_sum,
SUM(CASE WHEN rn % 2 = 0 THEN measurement_value else 0 end) as even_sum
FROM CTE
GROUP BY CAST(measurement_time AS DATE)
ORDER BY measurement_day
;
```
ROW_NUMBER() OVER(PARTITION BY EXTRACT(day FROM measurement_time) ORDER BY EXTRACT(hour FROM measurement_time)) function was used to ensure that we partitioned all the measurements by day and order them my time; this command will return the order (odd or even) of each measurement each day allowing for us to separate odd and even measurements. Next, we use the CTE to sum for the odd measurements and even measurements through CASE statement. 
