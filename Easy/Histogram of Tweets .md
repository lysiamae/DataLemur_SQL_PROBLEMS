# Histogram of Tweets 
#### [Easy]
  ---
Assume you're given a table Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group.

#### Table: tweets
|Column Name    | Type |
| ----------- | ----------- |
| tweet_id   | integer    |
| user_id | integer    |
| msg | string |
| tweet_date | timestamp|

####  tweets Example Input
|tweet_id    | user_id | msg | tweet_date |
| ----------- | ----------- | ----------- | ----------- |
| 214252  | 111   | Am considering taking Tesla private at $420. Funding secured. | 12/30/2021 00:00:00 |
| 739252 | 111 | Despite the constant negative press covfefe | 01/01/2022 00:00:00 |
| 846402 | 111 | Following @NickSinghTech on Twitter changed my life! | 02/14/2022 00:00:00 |
|241425 | 254 | If the salary is so competitive why won’t you tell me what it is? | 03/01/2022 00:00:00 |
| 231574 | 148  | I no longer have a manager. I can't be managed | 03/23/2022 00:00:00|


## Example Output:
| tweet_bucket | users_num |
| -----| ----| 
| 1 | 2 |
| 2 | 1 |

##### Explanation
Based on the example output, there are two users who posted only one tweet in 2022, and one user who posted two tweets in 2022. The query groups the users by the number of tweets they posted and displays the number of users in each group.

The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH CTE AS (SELECT user_id,count( distinct user_id)as user_num,count(tweet_id) as tweet_num  FROM tweets
where EXTRACT(year from tweet_date) = 2022
group by user_id)

SELECT tweet_num as tweet_bucket, sum(user_num) from cte
GROUP BY tweet_num;
```
First, we must find the total number of tweets made by each user in 2022 in this problem. To do this, we use COUNT and GROUP BY statements. Next, we must make sure that all data we have are for the year 2022 only. We use WHERE statement for this, we may approach this in two ways (1) extract the year from the tweet_date using EXTRACT() then equate it to 2022 to filter 2022 data only (2) we may use a between statement indicating tha the interval is between the first day of the year and the last day of the year. We then use this code as a subquery or CTE.

 
