# Histogram of Users and Purchases
#### [Medium]
  ---
Assume you're given a table on Walmart user transactions. Based on their most recent transaction date, write a query that retrieve the users along with the number of products they bought.

Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date.

#### Table: user_transactions 
|Column Name|	Type|
| ---- | ----|
|product_id|	integer|
|user_id|	integer|
|spend	|decimal|
|transaction_date|	timestamp|


#### user_transactions Example Input:
|product_id	|user_id|	spend|	transaction_date|
|---- | ---- | -----| ----|
|3673|	123|	68.90|	07/08/2022 12:00:00|
|9623|	123| 274.10|	07/08/2022 12:00:00|
|1467|	115	|19.90|	07/08/2022 12:00:00|
|2513|	159	|25.00|	07/08/2022 12:00:00|
|1452|	159|	74.50|	07/10/2022 12:00:00|



#### Example Output:
|measurement_day	|odd_sum|	even_sum|
| ----- | ---- | ----|
|07/10/2022 00:00:00|	2355.75|	1662.74|
|07/11/2022 00:00:00|	1124.50|	1234.14|


#### Example Output:
|transaction_date|	user_id|purchase_count|
|---|----| ----|
|07/08/2022 12:00:00|	115| 1|
|07/08/2022 12:00:000|	123	|2|
|07/10/2022 12:00:00|	159	|1|


##### Explanation
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH CTE AS (SELECT user_id, COUNT(product_id) as purchase_count, transaction_date, 
ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY transaction_date DESC) AS rn
FROM user_transactions
GROUP BY user_id, transaction_date)

SELECT transaction_date, user_id, purchase_count
FROM CTE 
WHERE rn = 1
ORDER BY transaction_date;
```
It is easier to order by the transaction_date in descending order in the ROW_NUMBER() statement because it will immediately assign 1 as the row number for the most recent transaction for all users. Meanwhile, if we order by ascending order it will be a challenge to filter all the recent transactions for all users, especially if the users have different number of transactions. 
