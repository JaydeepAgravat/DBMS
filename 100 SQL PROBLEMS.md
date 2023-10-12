# 100 SQL PROBLEMS

1. Print the title and ratings of the movies released in 2022 whose Metacritic rating is more than 60 and domestic collections exceed 10 crores.

    ```SQL
    SELECT Title, Rating FROM imdb
    NATURAL JOIN earning
    WHERE SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2012' AND MetaCritic > 60 AND Domestic > 100000000;
    ```

2. Print the genre and the maximum weighted rating among all the movies of that genre released in 2014 per genre.

    1. Do not print any row where either genre or the weighted rating is empty/null.
    2. weighted_rating = avgerge of (rating + metacritic/10.0)
    3. Keep the name of the columns as 'genre' and 'weighted_rating'
    4. The genres should be printed in alphabetical order.

    ```SQL
    SELECT T2.genre, MAX((T1.Rating+T1.MetaCritic/10)/2) AS  weighted_rating FROM imdb T1
    INNER JOIN genre T2
    ON T1.Movie_id = T2.Movie_id
    WHERE SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2014' AND T2.genre IS NOT NULL
    GROUP BY T2.genre
    ORDER BY T2.genre
    ```

3. From the IMDb dataset, print the title and rating of those movies which have a genre starting from 'C' released in 2014 with a budget higher than 4 Crore.

    ```SQL
    SELECT T1.title, T1.rating FROM imdb T1
    NATURAL JOIN genre T2
    WHERE  T2.genre LIKE 'C%' AND
    SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2014'  AND T1.Budget > 40000000    
    ```

4. Print the genre and the maximum net profit among all the movies of that genre released in 2012 per genre.

    1. Do not print any row where either genre or the net profit is empty/null.
    2. net_profit = Domestic + Worldwide - Budget
    3. Keep the name of the columns as 'genre' and 'net_profit'
    4. The genres should be printed in alphabetical order.

    ```SQL
    SELECT  T2.genre, MAX(T3.Domestic + T3.Worldwide - T1.Budget) AS net_profit FROM imdb T1
    NATURAL JOIN genre T2
    NATURAL JOIN earning T3
    WHERE T2.genre IS NOT NULL AND SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2012'
    GROUP BY T2.genre
    ORDER BY T2.genre
    ```

5. A country is big if it has an area of bigger than 3 million square km or a population of more than 25 million.
Write a SQL solution to output big countries' name, population and area.

    ```SQL
    SELECT name, population, area FROM world
    WHERE area > 3000000 OR population > 25000000
    ```
