# User's Third Transaction
#### [Easy]
  ---
Assume you are given the table below on Uber transactions made by users. Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date.

#### Table: transactions
|Column Name|	Type|
| ---- | ----|
|user_id|	integer|
|spend|	decimal|
|transaction_date|	timestamp|

#### transactions Example Input:
|user_id|	spend|	transaction_date|
|-----|-----|-----|
|111|	100.50|	01/08/2022 12:00:00|
|111|	55.00|	01/10/2022 12:00:00|
|121|	36.00|	01/18/2022 12:00:00|
|145|	24.99|	01/26/2022 12:00:00|
|111|	89.60|	02/05/2022 12:00:00|

#### Example Output:
|user_id|	spend|	transaction_date|
|-----|-----|-----|
|111	|89.60	|02/05/2022 12:00:00|


##### Explanation
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH T AS (SELECT 
  user_id, spend, transaction_date, 
  RANK() OVER(PARTITION BY user_id ORDER BY transaction_date) as rn
FROM transactions)

SELECT user_id, spend, transaction_date from T
where rn = 3;
```
The RANK() OVER() statement partitioned the transactions or entries by user_id and order it by the transaction date. We then use the query as CTE and simply obtain the third transaction for each user by looking for rows with rank 3.