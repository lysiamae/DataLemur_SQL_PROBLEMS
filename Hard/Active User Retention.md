# Active User Retention
#### [Hard]
  ---
Assume you're given a table containing information on Facebook user actions. Write a query to obtain number of monthly active users (MAUs) in July 2022, including the month in numerical format "1, 2, 3".

Hint:
- An active user is defined as a user who has performed actions such as 'sign-in', 'like', or 'comment' in both the current month and the previous month.

#### Table: user_actions 
|Column Name|	Type|
| ---- | ----|
|user_id|	integer|
|event_id|	integer|
|event_type|	string ("sign-in, "like", "comment")|
|event_date|	datetime|



#### user_actionsExample Input:
|user_id|	event_id|	event_type|	event_date|
|---|----|----|---|
|445	|7765	|sign-in|	05/31/2022 12:00:00|
|742|	6458|	sign-in|	06/03/2022 12:00:00|
|445|	3634|	like|	06/05/2022 12:00:00|
|742|	1374|	comment|	06/05/2022 12:00:00|
|648|	3124|	like|	06/18/2022 12:00:00|


#### Example Output for June 2022:
|month|	monthly_active_users|
| ---| ----|
|6|	1|


##### Explanation
In June 2022, there was only one monthly active user (MAU) with the user_id 445.

Please note that the output provided is for June 2022 as the user_actions table only contains event dates for that month. You should adapt the solution accordingly for July 2022.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution is shown below.
### SOLUTION: 
```sql
SELECT EXTRACT(MONTH FROM curr_month.event_date) as month, COUNT(DISTINCT curr_month.user_id)
FROM user_actions AS curr_month
WHERE EXISTS(SELECT last_month.user_id FROM user_actions as last_month
WHERE last_month.user_id = curr_month.user_id 
AND EXTRACT(MONTH FROM last_month.event_date)= EXTRACT(MONTH FROM curr_month.event_date)-1)
AND EXTRACT(MONTH FROM curr_month.event_date) =7 
and EXTRACT(YEAR FROM curr_month.event_date) = 2022
GROUP BY month;
```

To solve this problem, a query that allows to relate two sets of the same table was written. The two sets represent the Facebook users in the current month and last month.
To be exact, we aliased the user_actions table twice to use for the inner and outer query  which represents the current month and last month respectively.


