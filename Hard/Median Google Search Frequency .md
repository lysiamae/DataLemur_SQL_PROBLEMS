# Median Google Search Frequency 
#### [Hard]
  ---
Google's marketing team is making a Superbowl commercial and needs a simple statistic to put on their TV ad: the median number of searches a person made last year.

However, at Google scale, querying the 2 trillion searches is too costly. Luckily, you have access to the summary table which tells you the number of searches made last year and how many Google users fall into that bucket.
Write a query to report the median of searches made by a user. Round the median to one decimal point.

#### Table: search_frequency 
|Column Name|	Type|
| ----|---|
|searches|	integer|
|num_users|	integer|



#### search_frequency Example Input:
|searches|	num_users|
| ---|---|
|1|	2
|2|	2
|3|	3
|4|	1




#### Example Output:
|median|
|----|
|2.5|



##### Explanation
By expanding the search_frequency table, we get [1, 1, 2, 2, 3, 3, 3, 4] which has a median of 2.5 searches per user.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution is shown below.
### SOLUTION: 
```sql
WITH search AS (
SELECT searches
FROM search_frequency
GROUP BY searches,
  GENERATE_SERIES(1, num_users)
)

SELECT 
  PERCENTILE_CONT(0.5) 
    WITHIN GROUP (
      ORDER BY searches) AS median
FROM search;
```


