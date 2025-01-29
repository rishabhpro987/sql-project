# Spotify Advanced SQL Project
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:
- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.

### 3. Querying the Data
After the data is inserted, various SQL queries can be written to explore and analyze the data. Queries are categorized into **easy**, **medium**, and **advanced** levels to help progressively develop SQL proficiency.

#### Easy Queries
- Simple data retrieval, filtering, and basic aggregations.
  
#### Medium Queries
- More complex queries involving grouping, aggregation functions, and joins.
  
#### Advanced Queries
- Nested subqueries, window functions, CTEs, and performance optimization.
  
---

## 15 Practice Questions

### Easy Level
1. Retrieve the names of all tracks that have more than 1 billion streams.
```sql
SELECT ARTIST, TRACK, ALBUM, STREAM 
	FROM SPOTIFY
	WHERE STREAM >1000000000
	ORDER BY 1 ASC;
```
2. List all albums along with their respective artists.
```sql
SELECT DISTINCT ALBUM, ARTIST
	FROM SPOTIFY
	ORDER BY 1 ASC;
```
3. Get the total number of comments for tracks where `licensed = TRUE`.
```sql
SELECT LICENSCED, SUM(COMMENTS) AS "total comments"
	FROM SPOTIFY
	WHERE LICENSCED = TRUE
	GROUP BY LICENSCED
	;
```
4. Find all tracks that belong to the album type `single`.
```sql
SELECT track, album_type
	FROM SPOTIFY
	WHERE album_type= 'single'
	ORDER BY 1
```
5. Count the total number of tracks by each artist.
```sql
SELECT DISTINCT artist, COUNT(*) as total_tracks
	FROM SPOTIFY
	GROUP BY artist
	ORDER BY 2 desc;
```

### Medium Level
1. Calculate the average danceability of tracks in each album.
```sql
SELECT DISTINCT ALBUM, AVG(DANCEBLITY) AS average_danceablity
	FROM SPOTIFY
	GROUP BY ALBUM
	ORDER BY 2 DESC;
```
2. Find the top 5 tracks with the highest energy values.
```sql
SELECT track, MAX(energy) AS energy
	FROM SPOTIFY
	GROUP BY track
	ORDER BY 2 DESC
	LIMIT 5
```
3. List all tracks along with their views and likes where `official_video = TRUE`.
```sql
SELECT track, SUM(views), SUM(likes)
	FROM SPOTIFY
	WHERE official_video= 'TRUE'
	GROUP BY track
	ORDER BY 2 DESC;
```
4. For each album, calculate the total views of all associated tracks.
```sql
SELECT DISTINCT ALBUM, TRACK, SUM(VIEWS) 
	FROM SPOTIFY
	GROUP BY 1,2
	ORDER BY 3 DESC;
```
5. Retrieve the track names that have been streamed on Spotify more than YouTube.
```sql
SELECT * FROM
	(SELECT TRACK,
	--MOST_PLAYED_ON, 
	COALESCE(SUM(CASE WHEN MOST_PLAYED_ON= 'Youtube' THEN STREAM END),0) AS STREAM_ON_YOUTUBE,
	COALESCE(SUM(CASE WHEN MOST_PLAYED_ON= 'Spotify' THEN STREAM END),0) AS STREAM_ON_SPOTIFY
	FROM SPOTIFY
	GROUP BY 1) AS T1
	WHERE STREAM_ON_SPOTIFY > STREAM_ON_YOUTUBE
	AND STREAM_ON_YOUTUBE <> 0
	;
```

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
```sql
WITH RANKING_ARTIST 
	AS
	(SELECT ARTIST, TRACK, SUM(VIEWS),
	DENSE_RANK() OVER(PARTITION BY ARTIST ORDER BY SUM(VIEWS)DESC) AS RANK
	FROM SPOTIFY
	GROUP BY 1,2
	ORDER BY 1, 3 DESC
	)
	SELECT * FROM RANKING_ARTIST
	WHERE RANK <=3;
```
2. Write a query to find tracks where the liveness score is above the average.
```sql
SELECT TRACK, LIVENESS
	FROM SPOTIFY
	WHERE LIVENESS >= (SELECT AVG(LIVENESS) FROM SPOTIFY)
	ORDER BY 2 DESC;
```
3. **Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.**
```sql
WITH cte
AS
(SELECT 
	album,
	MAX(energy) as highest_energy,
	MIN(energy) as lowest_energery
FROM spotify
GROUP BY 1
)
SELECT 
	album,
	highest_energy - lowest_energery as energy_diff
FROM cte
ORDER BY 2 DESC
```
   
4. Find tracks where the energy-to-liveness ratio is greater than 1.2.
```sql
WITH CTE
	AS
	(SELECT TRACK, ENERGY / LIVENESS AS RATIO
	FROM SPOTIFY
	)
	SELECT TRACK, RATIO
	FROM CTE
	WHERE RATIO >= 1.2
	ORDER BY 2 DESC;
```
5. Calculate the cumulative sum of likes for tracks ordered by the number of views, using window functions.
```sql
SELECT TRACK,LIKES, SUM(LIKES) OVER (ORDER BY VIEWS DESC) AS CUMMILATIVE_SERIES
	FROM SPOTIFY
	ORDER BY 3;
```


Here’s an updated section for your **Spotify Advanced SQL Project** README, focusing on the query optimization task you performed. You can include the specific screenshots and graphs as described.

---

## How to Run the Project
1. Install PostgreSQL and pgAdmin (if not already installed).
2. Set up the database schema and tables using the provided normalization structure.
3. Insert the sample data into the respective tables.
4. Execute SQL queries to solve the listed problems.
5. Explore query optimization techniques for large datasets.

---

## License
This project is licensed under the MIT License.
