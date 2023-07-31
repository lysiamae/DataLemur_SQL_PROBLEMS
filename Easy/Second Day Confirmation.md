# Second Day Confirmation
#### [Easy]
  ---
Assume you're given tables with information about TikTok user sign-ups and confirmations through email and text. New users on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate their account.

Write a query to display the user IDs of those who did not confirm their sign-up on the first day, but confirmed on the second day.

Definition:
- action_date refers to the date when users activated their accounts and confirmed their sign-up through text messages.

#### Table: emails
|Column Name |	Type|
|----|---|
|email_id |	integer |
|user_id |	integer|
|signup_date|	datetime | 


####  emails Example Input
|email_id|	user_id |	signup_date |
| ----------- | ----------- | ----------- |
|125 |	7771|	06/14/2022 00:00:00|
|433 |	1052|	07/09/2022 00:00:00|

#### Table: texts
|Column Name |	Type |
| ----- | -----|
|text_id |	integer |
|email_id |	integer |
|signup_action|	string ('Confirmed', 'Not confirmed')|
|action_date |	datetime|

#### texts Example Input
|text_id |	email_id |	signup_action |	action_date |
| ----| ----| ----| ---- |
|6878 |	125 |	Confirmed |	06/14/2022 00:00:00 |
|6997 |	433 |	Not Confirmed |	07/09/2022 00:00:00 |
|7000 |	433	| Confirmed |	07/10/2022 00:00:00|

#### Example Output:
|user_id|
|---|
|1052|


##### Explanation
Only User 1052 confirmed their sign-up on the second day.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
SELECT e.user_id  FROM emails e
JOIN texts t
ON e.email_id = t.email_id
where t.signup_action = 'Confirmed' and 
 CAST(t.action_date AS DATE) = CAST(e.signup_date AS DATE) +1;
```
My approach in this problem is pretty straightforward. I used JOIN to obtain all rows from both tables where the key record of emails is equal to the key records of texts. The WHERE statement is then used to filter out results where signup action is "Confirmed" on exactly the "second" day of their initial sign up. 


