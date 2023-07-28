# Page With No Likes 
#### [Easy]
  ---
 
Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.
#### Table: pages
|Column Name     | Type |
| ----------- | ----------- |
| page_id     | integer      |
| page_name   | varchar     |

#### pages Example Input
|page_id     | page_name |
| ----------- | ----------- |
| 20001     | SQL Solutions      |
| 20045  | Brain Exercises     |
| 20701 | Tips for Data Analysts |

#### Table : page_likes 
|Column Name    | Type|
| ----------- | ----------- |
| user_id   | integer    |
| page_id  | integer   |
| liked_date | datetime |

####  page_likes Example Input
| user_id   | page_id | liked_date |
| ----------- | ----------- | ----------- |
| 111  | 20001   | 04/08/2022 00:00:00
| 121  | 20045   | 03/12/2022 00:00:00
| 156 | 20001 | 07/25/2022 00:00:00

## Example Output:
| page_id|
| -----|
| 20701|

The dataset you are querying against may have different input & output -
this is just an example!

My solution and comments are shown below.
### SOLUTION: 
```sql
SELECT p.page_id  
FROM pages p
LEFT JOIN page_likes pl         
ON  
  p.page_id = pl.page_id
WHERE 
  pl.liked_date is NULL
ORDER BY p.page_id;
```
 In this case we assigned pages as our left table, we used a LEFT JOIN to extract the left table's data only. Since left join not only combines the left table's rows but also the the rows that match alongside the right table, we put an argument "WHERE pl.liked_date is NULL" to obtain the data belonging to LEFT table that has no match on the right table. Through this we obtain page_ids of pages with no likes.

 