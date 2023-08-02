# Top 5 Artists
#### [Medium]
  ---
Assume there are three Spotify tables: artists, songs, and global_song_rank, which contain information about the artists, songs, and music charts, respectively.

Write a query to find the top 5 artists whose songs appear most frequently in the Top 10 of the global_song_rank table. Display the top 5 artist names in ascending order, along with their song appearance ranking.

Assumptions:
- If two or more artists have the same number of song appearances, they should be assigned the same ranking, and the rank numbers should be continuous (i.e. 1, 2, 2, 3, 4, 5).
- For instance, if both Ed Sheeran and Bad Bunny appear in the Top 10 five times, they should both be ranked 1st and the next artist should be ranked 2nd.

#### Table: artists
|Column Name|	Type|
|---|---|
|artist_id|	integer|
|artist_name|	varchar|


#### artists Example Input:
|artist_id|	artist_name|
|----|----|
|101|	Ed Sheeran|
|120|	Drake


#### Table: songs 
| Column Name | Type |
|----|---|
|song_id|	integer|
|artist_id|	integer|

#### songs Example Input:
|song_id|	artist_id|
|---|---|
|45202|	101|
|19960|	120|

#### Table: global_song_rank 
|Column Name|	Type|
|----|----|
|day|	integer (1-52)|
|song_id|	integer|
|rank|	integer (1-1,000,000)|

#### global_song_rank Example Input:
|day|	song_id|	rank|
|----|----|----|
|1	|45202|	5|
|3|	45202|	2|
|1	|19960	|3|
|9|	19960|	15|


#### Example Output:
|artist_name |	artist_rank |
| ---- |----|
|Ed Sheeran|	1|
|Drake|	2|

##### Explanation
Ed Sheeran's song appeared twice in the Top 10 list of global song rank while Drake's song is only listed once. Therefore, Ed is ranked #1 and Drake is ranked #2.
The dataset you are querying against may have different input & output - **this is just an example!**

My solution and comments are shown below.
### SOLUTION: 
```sql
WITH top AS (SELECT a.artist_name,
DENSE_RANK() OVER(ORDER BY COUNT(s.song_id) DESC) as ranking FROM artists a  
JOIN songs s 
ON a.artist_id = s.artist_id
JOIN global_song_rank g  
ON s.song_id = g.song_id 
WHERE g.rank <= 10
GROUP BY a.artist_name)

SELECT artist_name, ranking FROM top
WHERE ranking <= 5
ORDER BY ranking, artist_name;
```
DENSE_RANK() function is used to rank the artists based on their song appearances in the top 10 chart. We used DENSE_RANK() instead of RANK() since we want to ensure consecutive ranks even for tied values. In the CTE top we filter the songs ranking in top 10, and we used the cte top to filter the top 5 artist with most number of songs in the top 10.



