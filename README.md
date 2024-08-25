# MINIHACKATHON1.0

-- select movies which were released before 2000
SELECT 
Series_Title
 FROM
 imdb_top_1000
 AS Movies_released_before_2000
 WHERE
 Released_Year < 2000 ;


-- select top 10 movies which has the highest gross
SELECT
 Series_Title
 FROM
 imdb_top_1000
 ORDER BY
 Gross DESC LIMIT 10;


-- count the number of movies released before 2000
SELECT
 COUNT(Series_Title)
 FROM
 imdb_top_1000
 WHERE Released_Year<2000;


-- Top 5 directors with the most movies
SELECT
 COUNT(Director) AS DC ,Director
 FROM imdb_top_1000
 GROUP BY Director ORDER BY DC  DESC LIMIT 5;


-- Genre with the most movies
SELECT 
COUNT(GENRE) AS GC,GENRE FROM imdb_top_1000 
GROUP BY GENRE ORDER BY 
GC DESC LIMIT 1;


-- Movie with longest run time
SELECT 
Series_Title,run 
FROM 
imdb_top_1000 
WHERE 
Runtime =(SELECT MAX(RUNTIME) FROM imdb_top_1000);


-- average imdb rating of movies released after 2000
SELECT ROUND(AVG(IMDB_Rating),2) from imdb_top_1000 WHERE RELEASED_YEAR>2000;


-- top 3 highest grossing movie for each genre
WITH Movie_rank 
AS (SELECT Series_Title,Genre,Gross,row_number() OVER (partition by GENRE ORDER BY GROSS DESC) AS RN 
FROM imdb_top_1000)
SELECT Series_Title,Genre,Gross FROM MOVIE_RANK WHERE RN <=3 ORDER BY RN,GENRE;


-- number of movies released each year
SELECT Released_Year,COUNT(Series_Title)  OVER (PARTITION BY Released_Year) AS YEAR_COUNT FROM imdb_top_1000 ;


-- director who has the highest average imdb rating 
SELECT Director, AVG(IMDB_Rating) OVER(PARTITION BY Director) AS HIGHEST_AVG_IMDB_RATING FROM imdb_top_1000 ORDER BY HIGHEST_AVG_IMDB_RATING DESC LIMIT 1;


-- year in which most number of movies released
SELECT Released_Year,COUNT(Series_Title) OVER(PARTITION BY Released_Year) AS MC FROM imdb_top_1000 ORDER BY MC DESC LIMIT 1;


-- genre with the highest imdb rating
SELECT genre,MAX(IMDB_Rating) OVER (PARTITION BY Genre) AS GR FROM imdb_top_1000 ORDER BY GR DESC LIMIT 1;


-- actor who has acted in highest number of movies
WITH Movie_actors AS( 
SELECT Star1 AS Actor FROM imdb_top_1000 
UNION ALL 
SELECT Star2 AS Actor FROM imdb_top_1000 
UNION ALL 
SELECT Star3 AS Actor FROM imdb_top_1000
UNION ALL 
SELECT Star4 AS Actor FROM imdb_top_1000)
SELECT Actor,COUNT(ACTOR) AS Actor_Count FROM Movie_actors GROUP BY Actor ORDER BY Actor_Count DESC LIMIT 10;


-- year in which most number of movies released
SELECT Released_year, COUNT(Series_title) AS Year_count
FROM imdb_top_1000
GROUP BY Released_year
ORDER BY Year_count DESC
LIMIT 1;
