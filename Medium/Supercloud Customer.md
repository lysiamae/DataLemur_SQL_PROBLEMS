# Supercloud Customer
#### [Medium]
  ---
A Microsoft Azure Supercloud customer is defined as a company that purchases at least one product from each product category.

Write a query that effectively identifies the company ID of such Supercloud customers.

#### Table: customer_contracts 
|Column Name|	Type|
| ---- | ----|
|customer_id|	integer|
|product_id|	integer|
|amount|	integer|

#### customer_contracts Example Input:
|customer_id|	product_id|	amount|
| ---- | ---- | ---- |
|1|	1|	1000|
|1|	3|	2000|
|1|	5|	1500|
|2|	2|	3000|
|2|	6|	2000|

#### Table: products 
|Column Name |Type|
| --- |---|
|product_id	|integer|
|product_category|	string|
|product_name |	string|


#### products Example Input:
|product_id	|product_category|	product_name|
| ---- | ---- | ----|
|1|	Analytics|	Azure Databricks|
|2|	Analytics|	Azure Stream Analytics|
|4|	Containers|	Azure Kubernetes Service|
|5|	Containers|	Azure Service Fabric|
|6|	Compute	|Virtual Machines|
|7|	Compute	|Azure Functions|

#### Example Output:
|customer_id|
|----|
|1|

##### Explanation
Customer 1 bought from Analytics, Containers, and Compute categories of Azure, and thus is a Supercloud customer. Customer 2 isn't a Supercloud customer, since they don't buy any container services from Azure.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH CTE AS (SELECT c.customer_id, p.product_id, p.product_category 
FROM customer_contracts c
JOIN products p
ON c.product_id = p.product_id)

SELECT customer_id FROM CTE
GROUP BY customer_id
HAVING COUNT(DISTINCT product_category) = 3;
```
We used JOIN because we need the product category information alongside the customers information. COUNT DISTINCT statement was used to identify customers HAVING purchase from each product category.

