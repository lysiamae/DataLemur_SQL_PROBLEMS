# Compressed Mean
#### [Easy]
  ---
You're trying to find the mean number of items per order on Alibaba, rounded to 1 decimal place using tables which includes information on the count of items in each order (item_count table) and the corresponding number of orders for each item count (order_occurrences table).

#### Table:items_per_order 
|Column Name|	Type|
| ---- | ----|
|item_count | integer |
|order_occurrences |	integer|

#### items_per_order Example Input:
| item_count |	order_occurrences |
| ----| ----|
|1	|500 |
|2| 1000 |
|3	|800 |
|4 |1000|

#### Example Output:
|mean|
|---|
|2.7|

##### Explanation
Mean =  Total Items / Total Orders
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH table_order as (SELECT 
  (item_count * order_occurrences) as total_items, order_occurrences
FROM items_per_order)

SELECT 
  ROUND(CAST(SUM(total_items) AS DECIMAL) / SUM(order_occurrences),1)  
FROM table_order;
```
Note that it is important to use CAST() to convert the numerator in the mean formula into decimal in order to obtain a non-rounded down value, i.e. not converting the numerator into decimal form returns a rounded down value of 3 and not the actual mean which is approximately 3.87.
