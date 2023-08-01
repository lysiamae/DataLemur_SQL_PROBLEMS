# Sending vs. Opening Snaps
#### [Easy]
  ---
Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

Notes:
Calculate the following percentages:
- time spent sending / (Time spent sending + Time spent opening)
- Time spent opening / (Time spent sending + Time spent opening)
- To avoid integer division in percentages, multiply by 100.0 and not 100.


#### Table: activities
|Column Name|	Type|
| ---- | ----|
|activity_id |integer|
|user_id	|integer|
|activity_type|	string ('send', 'open', 'chat')|
|time_spent|	float|
|activity_date|	datetime|

#### activities Example Input:
|activity_id|	user_id	|activity_type|	time_spent|	activity_date|
|-----|-----|-----|----|----|
|7274|	123	|open|	4.50|	06/22/2022 12:00:00|
|2425|	123	|send|	3.50|	06/22/2022 12:00:00|
|1413|	456	|send|	5.67|	06/23/2022 12:00:00|
|1414|	789	|chat|	11.00|	06/25/2022 12:00:00|
|2536|	456|	open| 3.00|	06/25/2022 12:00:00|


#### Table: age_breakdown 
|Column Name|	Type|
|----|----|
|user_id|	integer|
|age_bucket|	string ('21-25', '26-30', '31-25')|

#### age_breakdown Example Input
|user_id|	age_bucket|
|----|----|
|123|	31-35|
|456|	26-30|
|789|	21-25|

#### Example Output
|age_bucket|	send_perc|	open_perc|
|-----|-----|-----|
|26-30|	65.40|	34.60|
|31-35|	43.75|	56.25|


##### Explanation
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH A AS (SELECT age_bucket,
  SUM(CASE WHEN activity_type = 'send' THEN time_spent END) as send_time,
  SUM(CASE WHEN activity_type = 'open' then time_spent END) as open_time
FROM activities
JOIN age_breakdown a
ON activities.user_id = a.user_id
GROUP BY age_bucket) 


SELECT  
    age_bucket,
    ROUND(CAST(send_time *100.0 / (send_time + open_time) AS NUMERIC), 2) AS send_perc,
    ROUND(CAST(open_time *100.0 / (send_time + open_time) AS NUMERIC), 2) AS open_perc
FROM A;
```
In the CTE (A) we obtain the total time spent in sending and total time spent in opening by using CASE() statement. GROUP BY  was also used to ensure that the output will show the send_time and open_time for each age_bucket. We then use A as CTE to calculate for the send percentage and open percentage noting that we should multiply by 100.0 to avoid integer division and cast the whole expression as numeric by using CAST().
