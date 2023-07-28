# Data Science Skills
#### [Easy]
  ---
Given a table of candidates and their skills, you're tasked with finding the candidates best suited for an open Data Science job. You want to find candidates who are proficient in Python, Tableau, and PostgreSQL.

Write a query to list the candidates who possess all of the required skills for the job. Sort the output by candidate ID in ascending order.
**Assumption:**
- There are no duplicates in the candidates table.

#### Table: candidates
|Column Name    | Type |
| ----------- | ----------- |
| candidate_id   | integer    |
| skill | varchar  |



#### candidates Example Input
| candidate_id    | skill|
| ----------- | ----------- | 
|123 | Python|
|123|	Tableau |
|123| PostgreSQL |
|234 |	R |
|234	|PowerBI |
|234|	SQL Server|
|345	|Python|
|345|	Tableau|


## Example Output:
| candidate_id | 
| -----| 
| 123|


##### Explanation
Candidate 123 is displayed because they have Python, Tableau, and PostgreSQL skills. 345 isn't included in the output because they're missing one of the required skills: PostgreSQL.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
SELECT 
    candidate_id 
FROM candidates
WHERE skill 
    IN ('Python','Tableau','PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(DISTINCT skill) = 3
ORDER BY candidate_id;
```
The HAVING statement makes sure that the output of the code will only contain IDs of the candidates who are proficient in the three skills desired. Without the HAVING statement, the code will also return the IDs of candidates who are only proficient in one or two of the skills desired.

