# International Call Percentage 
#### [Medium]
  ---
A phone call is considered an international call when the person calling is in a different country than the person receiving the call.
What percentage of phone calls are international? Round the result to 1 decimal.

Assumption:
- The caller_id in phone_info table refers to both the caller and receiver.

#### Table:phone_calls 
|Column Name |	Type|
| ---- | ---- |
|caller_id|	integer|
|receiver_id|	integer|
|call_time|	timestamp|


#### phone_calls Example Input:
|caller_id|	receiver_id| call_time|
| ----- | ----| -----|
|1|	2|	2022-07-04 10:13:49|
|1|	5|	2022-08-21 23:54:56|
|5|	1|	2022-05-13 17:24:06|
|5|	6|	2022-03-18 12:11:49|

#### Table:phone_info 
|Column Name |Type|
| ----|----|
|caller_id	|integer|
|country_id|	integer|
|network|	integer|
|phone_number|	string|


#### phone_info Example Input:
|caller_id |country_id|	network|	phone_number|
| ---- | ----| ----| ----|
|1|	US|	Verizon	|+1-212-897-1964|
|2|	US|	Verizon	|+1-703-346-9529|
|3|	US|	Verizon	|+1-650-828-4774|
|4|	US|	Verizon	|+1-415-224-6663|
|5|	IN|	Vodafone| +91 7503-907302|
|6|	IN|	Vodafone| +91 2287-664895|

#### Example Output:
|international_calls_pct|
|---|
|50.0|


##### Explanation
There is a total of 4 calls with 2 of them being international calls (from caller_id 1 => receiver_id 5, and caller_id 5 => receiver_id 1). Thus, 2/4 = 50.0%
The dataset you are querying against may have different input & output - **this is just an example!**

My solution is shown below.
### SOLUTION: 
```sql
WITH CALLS AS (SELECT 
  p1.country_id as caller_country,
  p2.country_id as receiver_country
FROM phone_calls p  
LEFT JOIN phone_info p1 
ON p.caller_id = p1.caller_id
LEFT JOIN phone_info p2 
ON p.receiver_id = p2.caller_id)

SELECT 
 ROUND(100.0*SUM(CASE WHEN receiver_country <> caller_country THEN 1 ELSE 0 END)  /
  COUNT(caller_country), 1) AS INTCALL_PERC
FROM CALLS;
```

We aliased the table phone_info twice to obtain the caller country and receiver country separately. The first aliasing was done to relate the caller id in table phone_calls to the caller id in table phone_info. The second aliasing was done to relate the receiver id in table phone_calls to the caller_id in the table phone_info. We see that we have to use LEFT JOIN to attain our goal to separate the receiver from the caller.
