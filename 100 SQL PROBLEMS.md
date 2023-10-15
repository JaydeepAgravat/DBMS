# 100 SQL PROBLEMS

1. Print the title and ratings of the movies released in 2022 whose Metacritic rating is more than 60 and domestic collections exceed 10 crores.

    ```SQL
    SELECT Title, Rating FROM imdb
    NATURAL JOIN earning
    WHERE SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2012'
    AND MetaCritic > 60 AND Domestic > 100000000;
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

6. Insert student details in students table and print all data of table.

    ```SQL
    INSERT INTO students (id, name, gender)
    VALUES
    (3,'Kim','F'),
    (4,'Molina','F'),
    (5,'Dev','M');

    SELECT * FROM students;
    ```

7. Write a SQL query to get the second highest salary from the Employee table.

    |  Id  |  Salary  |
    | ---  |  ------  |
    |  1   |   100    |
    |  2   |   200    |
    |  3   |   300    |

    For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.

    | SecondHighestSalary |
    | ------------------- |
    |         200         |

    ```SQL
    SELECT Salary FROM Employee
    ORDER BY Salary DESC
    LIMIT 1,1;
    ```

8. Write an SQL query to find all dates' id with higher temperature compared to its previous dates (yesterday). Return the result table in any order.

    Table: Weather

    |   Column Name   |   Type   |
    | --------------- | -------- |
    | id              | int      |
    | recordDate      | date     |
    | temperature     | int      |

    id is the primary key for this table.

    This table contains information about the temperature in a certain day.

    The query result format is in the following example:

    Weather:

    |  id  | recordDate | Temperature |
    | ---  | ---------- | ----------- |
    |  1   | 2015-01-01 |     10      |
    |  2   | 2015-01-02 |     25      |
    |  3   | 2015-01-03 |     20      |
    |  4   | 2015-01-04 |     30      |

    Result table:

    |  Id  |
    | ---  |
    |  2   |
    |  4   |

    ```SQL
    SELECT T.id
    FROM (
    SELECT *, (Temperature > LAG(Temperature) OVER (ORDER BY RecordDate)) AS isBig
    FROM weather
    ) AS T
    WHERE isBig = TRUE;
    ```

9. Given three tables: salesperson, company, orders. Output all the names in the table salesperson, who didn’t have sales to company 'RED'.

    Example

    Input

    Table: Salesperson

    | sales_id |  name  | salary | commission_rate | hire_date  |
    | -------- | ------ | ------ | --------------- | ---------- |
    |   1      | John   | 100000 | 6               | 4/1/2006   |
    |   2      | Amy    | 120000 | 5               | 5/1/2010   |
    |   3      | Mark   | 65000  | 12              | 12/25/2008 |
    |   4      | Pam    | 25000  | 25              | 1/1/2005   |
    |   5      | Alex   | 50000  | 10              | 2/3/2007   |

    The table salesperson holds the salesperson information. Every salesperson has a sales_id and a name.

    Table: Company

    | com_id |  name   | city     |
    | ------ | ------- | -------- |
    |   1    | RED     | Boston   |
    |   2    | ORANGE  | New York |
    |   3    | YELLOW  | Boston   |
    |   4    | GREEN   | Austin   |

    The table company holds the company information. Every company has a com_id and a name.

    Table: Orders

    | order_id | order_date | com_id | sales_id | amount  |
    | -------- | ---------- | ------ | -------- | ------- |
    | 1        | 1/1/2014   | 3      | 4        | 100000  |
    | 2        | 2/1/2014   | 4      | 5        | 5000    |
    | 3        | 3/1/2014   | 1      | 1        | 50000   |
    | 4        | 4/1/2014   | 1      | 4        | 25000   |

    The table orders holds the sales record information, salesperson and customer company are represented by sales_id and com_id.

    Output

    | name |
    | ---- |
    | Amy  |
    | Mark |
    | Alex |

    ```SQL
    SELECT SalesPerson.name
    FROM SalesPerson
    LEFT JOIN (
        SELECT DISTINCT Orders.sales_id
        FROM Company
        JOIN Orders ON Company.com_id = Orders.com_id
        WHERE Company.name = 'RED'
    ) RedSales ON SalesPerson.sales_id = RedSales.sales_id
    WHERE RedSales.sales_id IS NULL;
    ```

10. Write a SQL query for a report that provides the pairs (actor_id, director_id) where the actor has co-worked with the director for 3 times.

    | Column Name | Type    |
    |-------------|---------|
    | actor_id    | int     |
    | director_id | int     |
    | timestamp   | int     |

    Timestamp is the primary key column for this table.

    Example

    ActorDirector table:

    | actor_id | director_id | timestamp |
    |---------|-------------|-----------|
    | 1       | 1           | 0         |
    | 1       | 1           | 1         |
    | 1       | 1           | 2         |
    | 1       | 2           | 3         |
    | 1       | 2           | 4         |
    | 2       | 1           | 5         |
    | 2       | 1           | 6         |

    Result table:

    | actor_id | director_id |
    |---------|-------------|
    | 1       | 1           |

    ```SQL
    SELECT actor_id, director_id From actordirector
    GROUP BY actor_id, director_id
    HAVING COUNT(*) = 3
    ```

11. swap sex

    id is the primary key for this table.
    The sex column is ENUM value of type ('m', 'f').
    The table contains information about an employee.

    Table: Salary

    | Column Name | Type     |
    |------------- |----------|
    | id          | int      |
    | name        | varchar  |
    | sex         | ENUM     |
    | salary      | int      |

    ```SQL
    UPDATE salary SET sex = REPLACE ('fm', sex, ''); 
    ```

12. Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

    | Id | Score |
    |----|-------|
    | 1  | 3.50  |
    | 2  | 3.65  |
    | 3  | 4.00  |
    | 4  | 3.85  |
    | 5  | 4.00  |
    | 6  | 3.65  |

    For example, given the above Scores table, your query should generate the following report (order by highest score):

    | Score | Rank |
    |-------|------|
    | 4.00  | 1    |
    | 4.00  | 1    |
    | 3.85  | 2    |
    | 3.65  | 3    |
    | 3.65  | 3    |
    | 3.50  | 4    |

    ```SQL
    SELECT score,
    DENSE_RANK() OVER(ORDER BY score DESC) AS "RANK"
    FROM scores ;
    ```

13. Write an SQL query to find all numbers that appear at least three times consecutively. Return the result table in any order.

    Table: Logs

    | Column Name | Type    |
    |-------------|---------|
    | id          | int     |
    | num         | varchar |

    The query result format is in the following example:

    Table: Logs

    | Id | Num |
    |----|-----|
    | 1  | 1   |
    | 2  | 1   |
    | 3  | 1   |
    | 4  | 2   |
    | 5  | 1   |
    | 6  | 2   |
    | 7  | 2   |

    Result table:

    | ConsecutiveNums |
    |-----------------|
    | 1               |

    1 is the only number that appears consecutively for at least three times.

    ```SQL
    SELECT Num AS ConsecutiveNums
    FROM (
        SELECT *,
            LAG(Num) OVER() AS c1,
            LAG(Num, 2) OVER() AS c2
        FROM logs
    ) AS subquery
    WHERE c1 = c2 AND num = c1;
    ```

14. Write an SQL query to report the number of calls and the total call duration between each pair of distinct persons (person1, person2) where person1 != person2. Return the result table in any order.

    Table: Calls

    | Column Name | Type    |
    |-------------|---------|
    | from_id     | int     |
    | to_id       | int     |
    | duration    | int     |

    This table does not have a primary key, it may contain duplicates.
    This table contains the duration of a phone call between from_id and to_id.
    from_id != to_id

    The query result format is in the following example:

    Calls table:

    | from_id | to_id | duration |
    |---------|-------|----------|
    | 1       | 2     | 59       |
    | 2       | 1     | 11       |
    | 1       | 3     | 20       |
    | 3       | 4     | 100      |
    | 3       | 4     | 200      |
    | 3       | 4     | 200      |
    | 4       | 3     | 499      |

    Result table:

    | person1 | person2 | call_count | total_duration |
    |---------|---------|------------|----------------|
    | 1       | 2       | 2          | 70             |
    | 1       | 3       | 1          | 20             |
    | 3       | 4       | 4          | 999            |

    Users 1 and 2 had 2 calls and the total duration is 70 (59 + 11).
    Users 1 and 3 had 1 call and the total duration is 20.
    Users 3 and 4 had 4 calls and the total duration is 999 (100 + 200 + 200 + 499).

    ```SQL
    SELECT
    CASE
        WHEN from_id <= to_id THEN from_id
        ELSE to_id
    END AS person1,
    CASE
        WHEN from_id <= to_id THEN to_id
        ELSE from_id
    END AS person2,
    COUNT(*) AS call_count,
    SUM(duration) AS total_duration
    FROM
        calls
    GROUP BY
        person1,
        person2;
    ```

15. Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people: FirstName, LastName, City, State

    Table: Person

    | Column Name | Type    |
    |-------------|---------|
    | PersonId    | int     |
    | FirstName   | varchar |
    | LastName    | varchar |

    PersonId is the primary key column for this table.
    Table: Address

    | Column Name | Type    |
    |-------------|---------|
    | AddressId   | int     |
    | PersonId    | int     |
    | City        | varchar |
    | State       | varchar |

    AddressId is the primary key column for this table.

    ```SQL
    SELECT FirstName, LastName, City, State FROM Person
    LEFT JOIN Address
    ON Person.PersonId = Address.PersonId
    ```

16. D

    ```SQL
    
    ```

17. D

    ```SQL
    
    ```

18. D

    ```SQL
    
    ```

19. D

    ```SQL
    
    ```

20. D

    ```SQL

    ```
