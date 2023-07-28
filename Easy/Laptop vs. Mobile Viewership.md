# Laptop vs. Mobile Viewership
#### [Easy]
  ---
Assume you're given the table on user viewership categorised by device type where the three types are laptop, tablet, and phone.

Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as laptop_reviews and the total viewership for mobile devices as mobile_views.

#### Table: tweets
|Column Name    | Type |
| ----------- | ----------- |
| user_id   | integer    |
| device_type | integer    |
| view_time | timestamp |


####  page_likes Example Input
| user_id    | device_type | view_time | 
| ----------- | ----------- | ----------- |
|123|	tablet|	01/02/2022 00:00:00 |
|125 | laptop |	01/07/2022 00:00:00 |
|128 |	laptop | 02/09/2022 00:00:00 |
|129 |	phone |	02/09/2022 00:00:00 |
|145 |	tablet | 02/24/2022 00:00:00 |

## Example Output:
| laptop_views | mobile_views |
| -----| ----| 
| 2 | 3 |


##### Explanation
Based on the example input, there are a total of 2 laptop views and 3 mobile views.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
SELECT 
SUM(CASE WHEN device_type ='laptop' then 1 else 0 end) as laptop_views,
SUM(CASE WHEN device_type in ('tablet','phone') then 1 else 0 end) as mobile_views
from viewership;
```
The solution to this problem is pretty straightforward. We use case statements to count the laptop and mobile views respectively. For instance, for laptop views a value of 1 will be returned if the device used is laptop if not then value returned is 0. We then use sum to count the number of instances laptop is used as a device.
 
