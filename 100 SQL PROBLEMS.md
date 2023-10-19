# 100 SQL PROBLEMS

1. Print the title and ratings of the movies released in 2022 whose Metacritic rating is more than 60 and domestic collections exceed 10 crores.

    ```SQL
    SELECT Title, Rating FROM imdb
    NATURAL JOIN earning
    WHERE SUBSTRING(Title, LOCATE('(', Title) + 1, 4) = '2012'
    AND MetaCritic > 60 AND Domestic > 100000000
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

    SELECT * FROM students
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
    LIMIT 1,1
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
    WHERE isBig = TRUE
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
    WHERE RedSales.sales_id IS NULL
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
    UPDATE salary SET sex = REPLACE ('fm', sex, '')
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
    FROM scores 
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
    WHERE c1 = c2 AND num = c1
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
        person2
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

16. Table: Warehouse

    | Column Name  | Type    |
    | ------------- | ------- |
    | name         | varchar |
    | product_id   | int     |
    | units        | int     |

    (name, product_id) is the primary key for this table.
    Each row of this table contains the information of the products in each warehouse.

    Table: Products

    | Column Name   | Type    |
    | ------------- | ------- |
    | product_id    | int     |
    | product_name  | varchar |
    | Width         | int     |
    | Length        | int     |
    | Height        | int     |

    product_id is the primary key for this table.
    Each row of this table contains the information about the product dimensions (Width, Lenght and Height) in feets of each product.

    Write an SQL query to report, How much cubic feet of volume does the inventory occupy in each warehouse.

    warehouse_name
    volume
    Return the result table in any order.

    The query result format is in the following example.

    Warehouse table:

    | name     | product_id | units |
    | -------- | ---------- | ----- |
    | LCHouse1 | 1          | 1     |
    | LCHouse1 | 2          | 10    |
    | LCHouse1 | 3          | 5     |
    | LCHouse2 | 1          | 2     |
    | LCHouse2 | 2          | 2     |
    | LCHouse3 | 4          | 1     |

    Products table:

    | product_id | product_name  | Width | Length | Height |
    | ---------- | ------------- | ----- | ------ | ------ |
    | 1          | LC-TV         | 5     | 50     | 40     |
    | 2          | LC-KeyChain   | 5     | 5      | 5      |
    | 3          | LC-Phone      | 2     | 10     | 10     |
    | 4          | LC-T-Shirt    | 4     | 10     | 20     |

    Result table:

    | warehouse_name | volume |
    | --------------- | ------ |
    | LCHouse1       | 12250  |
    | LCHouse2       | 20250  |
    | LCHouse3       | 800    |

    Volume of product_id = 1 (LC-TV), 5x50x40 = 10000

    Volume of product_id = 2 (LC-KeyChain), 5x5x5 = 125

    Volume of product_id = 3 (LC-Phone), 2x10x10 = 200

    Volume of product_id = 4 (LC-T-Shirt), 4x10x20 = 800

    LCHouse1: 1 unit of LC-TV + 10 units of
    LC-KeyChain + 5 units of LC-Phone.

    Total volume: 1*10000 + 10*125  + 5*200 = 12250 cubic feet

    LCHouse2: 2 units of LC-TV + 2 units of LC-KeyChain.

    Total volume: 2*10000 + 2*125 = 20250 cubic feet

    LCHouse3: 1 unit of LC-T-Shirt.

    Total volume: 1*800 = 800 cubic feet.

    ```SQL
    SELECT warehouse.name AS warehouse_name,SUM(warehouse.units*products.width*products.length*products.height) AS volume
    FROM warehouse
    LEFT JOIN products
    ON warehouse.product_id = products.product_id
    GROUP BY warehouse.name
    ```

17. Write an SQL query to find all the people who viewed more than one article on the same date, sorted in ascending order by their id.

    Table: Views

    | Column Name | Type  |
    | ----------- | ----- |
    | article_id  | int   |
    | author_id   | int   |
    | viewer_id   | int   |
    | view_date   | date  |

    There is no primary key for this table, it may have duplicate rows.
    Each row of this table indicates that some viewer viewed an article (written by some author) on some date.
    Note that equal author_id and viewer_id indicate the same person.

    The query result format is in the following example:

    Views table:

    | article_id | author_id | viewer_id | view_date  |
    | ---------- | --------- | --------- | ---------- |
    | 1          | 3         | 5         | 2019-08-01 |
    | 3          | 4         | 5         | 2019-08-01 |
    | 1          | 3         | 6         | 2019-08-02 |
    | 2          | 7         | 7         | 2019-08-01 |
    | 2          | 7         | 6         | 2019-08-02 |
    | 4          | 7         | 1         | 2019-07-22 |
    | 3          | 4         | 4         | 2019-07-21 |
    | 3          | 4         | 4         | 2019-07-21 |

    Result table:

    | id |
    | -- |
    | 5  |
    | 6  |

    ```SQL
    SELECT viewer_id AS id FROM Views 
    GROUP BY viewer_id
    HAVING count(distinct article_id)>1 
    ```

18. Query all columns for all Marvel cities in the CITY table with populations larger than 100000. The CountryCode for Marvel is Marv.

    The CITY table is described as follows:

    | Field        | Type    |
    | ------------ | ------- |
    | ID           | Number  |
    | Name         | Varchar |
    | CountryCode  | Varchar |
    | Population   | Number  |

    ```SQL
    SELECT * FROM CITY
    WHERE population > 100000 AND
    countrycode = 'Marv'
    ```

19. Write an SQL query to report all the sessions that did not get shown any ads.

    Table: Playback

    | Column Name | Type |
    | ----------- | ---- |
    | session_id  | int  |
    | customer_id | int  |
    | start_time  | int  |
    | end_time    | int  |

    session_id is the primary key for this table.
    customer_id is the ID of the customer watching this session.
    The session runs during the inclusive interval between start_time and end_time.
    It is guaranteed that start_time <= end_time and that two sessions for the same customer do not intersect.

    Table: Ads

    | Column Name | Type |
    | ----------- | ---- |
    | ad_id       | int  |
    | customer_id | int  |
    | timestamp   | int  |

    ad_id is the primary key for this table.
    customer_id is the ID of the customer viewing this ad.
    timestamp is the moment of time at which the ad was shown.

    Return the result table in any order.

    The query result format is in the following example:

    Playback table:

    | session_id | customer_id | start_time | end_time |
    | ---------- | ----------- | ---------- | -------- |
    | 1          | 1           | 1          | 5        |
    | 2          | 1           | 15         | 23       |
    | 3          | 2           | 10         | 12       |
    | 4          | 2           | 17         | 28       |
    | 5          | 2           | 2          | 8        |

    Ads table:

    | ad_id | customer_id | timestamp |
    | ----- | ----------- | --------- |
    | 1     | 1           | 5         |
    | 2     | 2           | 17        |
    | 3     | 2           | 20        |

    Result table:

    | session_id |
    | ---------- |
    | 2          |
    | 3          |
    | 5          |

    The ad with ID 1 was shown to user 1 at time 5 while they were in session 1.

    The ad with ID 2 was shown to user 2 at time 17 while they were in session 4.

    The ad with ID 3 was shown to user 2 at time 20 while they were in session 4.

    We can see that sessions 1 and 4 had at least one ad. Sessions 2, 3, and 5 did not have any ads, so we return them.

    ```SQL
    SELECT DISTINCT session_id FROM playback 
    NATURAL JOIN ads
    WHERE timestamp < start_time
    OR timestamp > end_time
    ORDER BY session_id
    ```

20. Write an SQL query to find the ids of products that are both low fat and recyclable.

    Table: Products

    | Column Name | Type  |
    | ----------- | ----- |
    | product_id  | int   |
    | low_fats    | enum  |
    | recyclable  | enum  |

    product_id is the primary key for this table.

    low_fats is an ENUM of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.

    recyclable is an ENUM of types ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.

    Return the result table in any order.

    The query result format is in the following example:

    Products table:

    | product_id  | low_fats | recyclable |
    | ----------- | -------- | ---------- |
    | 0           | Y        | N          |
    | 1           | Y        | Y          |
    | 2           | N        | Y          |
    | 3           | Y        | Y          |
    | 4           | N        | N          |

    Result table:

    | product_id  |
    | ----------- |
    | 1           |
    | 3           |

    Only products 1 and 3 are both low fat and recyclable.

    ```SQL
    SELECT product_id FROM products
    WHERE low_fats='Y' and recyclable='Y'
    ```

21. Write a SQL query to find all duplicate emails in a table named Person.

    | Id | Email   |
    | -- | ------- |
    | 1  | ab.com |
    | 2  | cd.com |
    | 3  | ab.com |

    For example, your query should return the following for the above table:

    | Email   |
    | ------- |
    | ab.com |

    ```SQL
    SELECT Email FROM Person
    GROUP BY Email
    HAVING COUNT(*)>1
    ```

22. Write an SQL query to find the customer_number for the customer who has placed the largest number of orders.

    Table: Orders

    | Column Name     | Type |
    | --------------- | ---- |
    | order_number    | int  |
    | customer_number | int  |

    order_number is the primary key for this table.
    This table contains information about the order ID and the customer ID.

    It is guaranteed that exactly one customer will have placed more orders than any other customer.

    The query result format is in the following example:

    Orders table:

    | order_number | customer_number |
    | ------------ | ---------------- |
    | 1            | 1                |
    | 2            | 2                |
    | 3            | 3                |
    | 4            | 3                |

    Result table:

    | customer_number |
    | ---------------- |
    | 3               |

    The customer with number 3 has two orders, which is greater than either customer 1 or 2 because each of them only has one order.
    So the result is customer_number 3.

    ```SQL
    SELECT customer_number FROM Orders
    GROUP BY customer_number
    ORDER BY COUNT(*) DESC
    LIMIT 1
    ```

23. A pupil Tim gets homework to identify whether three line segments could possibly form a triangle.

    However, this assignment is very heavy because there are hundreds of records to calculate.

    Could you help Tim by writing a query to judge whether these three  sides can form a triangle, assuming table triangle holds the length of the three sides x, y and z.

    |  x  |  y  |  z  |
    |----|----|----|
    | 13 | 15 | 30 |
    | 10 | 20 | 15 |

    For the sample data above, your query should return the follow result:

    |  x  |  y  |  z  | triangle |
    |----|----|----|----------|
    | 13 | 15 | 30 | No       |
    | 10 | 20 | 15 | Yes      |

    ```SQL
    SELECT x, y, z,
    CASE
        WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
        ELSE 'No'
    END AS triangle
    FROM triangle
    ```

24. Write an SQL query to find the most frequently ordered product(s) for each customer.

    Table: Customers

    | Column Name   | Type    |
    | ------------- | ------- |
    | customer_id   | int     |
    | name          | varchar |

    customer_id is the primary key for this table.
    This table contains information about the customers.

    Table: Orders

    | Column Name   | Type  |
    | ------------- | ----- |
    | order_id      | int   |
    | order_date    | date  |
    | customer_id   | int   |
    | product_id    | int   |

    order_id is the primary key for this table.
    This table contains information about the orders made by customer_id.
    No customer will order the same product more than once in a single day.

    Table: Products

    | Column Name   | Type    |
    | ------------- | ------- |
    | product_id    | int     |
    | product_name  | varchar |
    | price         | int     |

    product_id is the primary key for this table.
    This table contains information about the products.

    The result table should have the product_id and product_name for each customer_id who ordered at least one order. Return the result table in any order.

    The query result format is in the following example:

    Customers

    | customer_id | name  |
    | ----------- | ----- |
    | 1           | Alice |
    | 2           | Bob   |
    | 3           | Tom   |
    | 4           | Jerry |
    | 5           | John  |

    Orders

    | order_id | order_date | customer_id | product_id |
    | -------- | ---------- | ----------- | ---------- |
    | 1        | 2020-07-31 | 1           | 1          |
    | 2        | 2020-07-30 | 2           | 2          |
    | 3        | 2020-08-29 | 3           | 3          |
    | 4        | 2020-07-29 | 4           | 1          |
    | 5        | 2020-06-10 | 1           | 2          |
    | 6        | 2020-08-01 | 2           | 1          |
    | 7        | 2020-08-01 | 3           | 3          |
    | 8        | 2020-08-03 | 1           | 2          |
    | 9        | 2020-08-07 | 2           | 3          |
    | 10       | 2020-07-15 | 1           | 2          |

    Products

    | product_id | product_name | price |
    | ---------- | ------------ | ----- |
    | 1          | keyboard     | 120   |
    | 2          | mouse        | 80    |
    | 3          | screen       | 600   |
    | 4          | hard disk    | 450   |

    Result table:

    | customer_id | product_id | product_name |
    | ----------- | ---------- | ------------ |
    | 1           | 2          | mouse        |
    | 2           | 1          | keyboard     |
    | 2           | 2          | mouse        |
    | 2           | 3          | screen       |
    | 3           | 3          | screen       |
    | 4           | 1          | keyboard     |

    Alice (customer 1) ordered the mouse three times and the keyboard one time, so the mouse is the most frquently ordered product for them.

    Bob (customer 2) ordered the keyboard, the mouse, and the screen one time, so those are the most frquently ordered products for them.

    Tom (customer 3) only ordered the screen (two times), so that is the most frquently ordered product for them.

    Jerry (customer 4) only ordered the keyboard (one time), so that is the most frquently ordered product for them.

    John (customer 5) did not order anything, so we do not include them in the result table.

    ```SQL
    WITH ProductCounts AS (
    SELECT
        o.customer_id,
        o.product_id,
        p.product_name,
        COUNT(*) AS order_count
    FROM Orders o
    JOIN Products p ON o.product_id = p.product_id
    GROUP BY o.customer_id, o.product_id, p.product_name
    )

    SELECT customer_id, product_id, product_name
    FROM (
    SELECT
        customer_id,
        product_id,
        product_name,
        RANK() OVER (PARTITION BY customer_id ORDER BY order_count DESC) AS rnk
    FROM ProductCounts
    ) AS ranked
    WHERE rnk = 1
    ORDER BY customer_id, product_id
    ```

25. Write an SQL query to find the npv of all each query of queries table.

    Table: NPV

    | Column Name | Type |
    | ----------- | ---- |
    | id          | int  |
    | year        | int  |
    | npv         | int  |

    (id, year) is the primary key of this table.
    The table has information about the id and the year of each inventory and the corresponding net present value.

    Table: Queries

    | Column Name | Type |
    | ----------- | ---- |
    | id          | int  |
    | year        | int  |

    (id, year) is the primary key of this table.
    The table has information about the id and the year of each inventory query.

    Return the result table in any order.

    The query result format is in the following example:

    NPV table:

    | id   | year | npv |
    | ---- | ---- | --- |
    | 1    | 2018 | 100 |
    | 7    | 2020 | 30  |
    | 13   | 2019 | 40  |
    | 1    | 2019 | 113 |
    | 2    | 2008 | 121 |
    | 3    | 2009 | 12  |
    | 11   | 2020 | 99  |
    | 7    | 2019 | 0   |

    Queries table:

    | id   | year |
    | ---- | ---- |
    | 1    | 2019 |
    | 2    | 2008 |
    | 3    | 2009 |
    | 7    | 2018 |
    | 7    | 2019 |
    | 7    | 2020 |
    | 13   | 2019 |

    Result table:

    | id   | year | npv |
    | ---- | ---- | --- |
    | 1    | 2019 | 113 |
    | 2    | 2008 | 121 |
    | 3    | 2009 | 12  |
    | 7    | 2018 | 0   |
    | 7    | 2019 | 0   |
    | 7    | 2020 | 30  |
    | 13   | 2019 | 40  |

    The npv value of (7, 2018) is not present in the NPV table, we consider it 0.

    The npv values of all other queries can be found in the NPV table.

    ```SQL
    SELECT queries.id, queries.year,
    COALESCE(npv.npv, 0) AS npv
    FROM queries
    LEFT JOIN npv ON queries.id = npv.id AND queries.year = npv.year
    ```

26. Write an SQL query to find the order_id of all imbalanced orders.

    Table: OrdersDetails

    | Column Name | Type |
    | ----------- | ---- |
    | order_id    | int  |
    | product_id  | int  |
    | quantity    | int  |

    (order_id, product_id) is the primary key for this table.
    A single order is represented as multiple rows, one row for each product in the order.
    Each row of this table contains the quantity ordered of the product product_id in the order order_id.

    You are running an ecommerce site that is looking for imbalanced orders. An imbalanced order is one whose maximum quantity is strictly greater than the average quantity of every order (including itself).

    The average quantity of an order is calculated as (total quantity of all products in the order) / (number of different products in the order). The maximum quantity of an order is the highest quantity of any single product in the order.

    Return the result table in any order.

    The query result format is in the following example:

    OrdersDetails table:

    | order_id | product_id | quantity |
    | -------- | ---------- | -------- |
    | 1        | 1          | 12       |
    | 1        | 2          | 10       |
    | 1        | 3          | 15       |
    | 2        | 1          | 8        |
    | 2        | 4          | 4        |
    | 2        | 5          | 6        |
    | 3        | 3          | 5        |
    | 3        | 4          | 18       |
    | 4        | 5          | 2        |
    | 4        | 6          | 8        |
    | 5        | 7          | 9        |
    | 5        | 8          | 9        |
    | 3        | 9          | 20       |
    | 2        | 9          | 4        |

    Result table:

    | order_id |
    | -------- |
    | 1        |
    | 3        |

    The average quantity of each order is:

    - order_id=1: (12+10+15)/3 = 12.3333333
    - order_id=2: (8+4+6+4)/4 = 5.5
    - order_id=3: (5+18+20)/3 = 14.333333
    - order_id=4: (2+8)/2 = 5
    - order_id=5: (9+9)/2 = 9

    The maximum quantity of each order is:

    - order_id=1: max(12, 10, 15) = 15
    - order_id=2: max(8, 4, 6, 4) = 8
    - order_id=3: max(5, 18, 20) = 20
    - order_id=4: max(2, 8) = 8
    - order_id=5: max(9, 9) = 9

    Orders 1 and 3 are imbalanced because they have a maximum quantity that exceeds the average quantity of every order.

    ```SQL
    SELECT order_id
    FROM OrdersDetails
    GROUP BY order_id
    HAVING MAX(quantity) > ALL 
    ( SELECT AVG(quantity) FROM OrdersDetails
      GROUP BY order_id
    );
    ```

27. Write an SQL query that will, for all products, return each product name with total amount due, paid, canceled, and refunded across all invoices.

    Table: Product

    | Column Name | Type    |
    | ----------- | ------- |
    | product_id  | int     |
    | name        | varchar |

    product_id is the primary key for this table.
    This table contains the ID and the name of the product. The name consists of only lowercase English letters. No two products have the same name.

    Table: Invoice

    | Column Name | Type |
    | ----------- | ---- |
    | invoice_id  | int  |
    | product_id  | int  |
    | rest        | int  |
    | paid        | int  |
    | canceled    | int  |
    | refunded    | int  |

    invoice_id is the primary key for this table and the id of this invoice.
    product_id is the id of the product for this invoice.
    rest is the amount left to pay for this invoice.
    paid is the amount paid for this invoice.
    canceled is the amount canceled for this invoice.
    refunded is the amount refunded for this invoice.

    Return the result table ordered by product_name.

    The query result format is in the following example:

    Product table:

    | product_id | name  |
    | ---------- | ----- |
    | 0          | ham   |
    | 1          | bacon |

    Invoice table:

    | invoice_id | product_id | rest | paid | canceled | refunded |
    | ---------- | ---------- | ---- | ---- | -------- | -------- |
    | 23         | 0          | 2    | 0    | 5        | 0        |
    | 12         | 0          | 0    | 4    | 0        | 3        |
    | 1          | 1          | 1    | 1    | 0        | 1        |
    | 2          | 1          | 1    | 0    | 1        | 1        |
    | 3          | 1          | 0    | 1    | 1        | 1        |
    | 4          | 1          | 1    | 1    | 1        | 0        |

    Result table:

    | name  | rest | paid | canceled | refunded |
    | ----- | ---- | ---- | -------- | -------- |
    | bacon | 3    | 3    | 3        | 3        |
    | ham   | 2    | 4    | 5        | 3        |

    - The amount of money left to pay for bacon is 1 + 1 + 0 + 1 = 3
    - The amount of money paid for bacon is 1 + 0 + 1 + 1 = 3
    - The amount of money canceled for bacon is 0 + 1 + 1 + 1 = 3
    - The amount of money refunded for bacon is 1 + 1 + 1 + 0 = 3
    - The amount of money left to pay for ham is 2 + 0 = 2
    - The amount of money paid for ham is 0 + 4 = 4
    - The amount of money canceled for ham is 5 + 0 = 5
    - The amount of money refunded for ham is 0 + 3 = 3

    ```SQL
    SELECT Product.name,
       SUM(Invoice.rest) AS rest,
       SUM(Invoice.paid) AS paid,
       SUM(Invoice.canceled) AS canceled,
       SUM(Invoice.refunded) AS refunded
    FROM Product
    LEFT JOIN Invoice
    ON Product.product_id = Invoice.product_id
    GROUP BY Product.product_id, Product.name
    ORDER BY Product.name
    ```

28. Write an SQL query to report all the sessions that did not get shown any ads.

    Table: Playback

    | Column Name | Type |
    | ----------- | ---- |
    | session_id  | int  |
    | customer_id | int  |
    | start_time  | int  |
    | end_time    | int  |

    session_id is the primary key for this table.

    customer_id is the ID of the customer watching this session.

    The session runs during the inclusive interval between start_time and end_time.

    It is guaranteed that start_time &lt;= end_time and that two sessions for the same customer do not intersect.

    Table: Ads

    | Column Name | Type |
    | ----------- | ---- |
    | ad_id       | int  |
    | customer_id | int  |
    | timestamp   | int  |

    ad_id is the primary key for this table.

    Customer_id is the ID of the customer viewing this ad.

    Timestamp is the moment of time at which the ad was shown.

    Return the result table in any order.

    The query result format is in the following example:

    Playback table:

    | session_id | customer_id | start_time | end_time |
    | ---------- | ----------- | ---------- | -------- |
    | 1          | 1           | 1          | 5        |
    | 2          | 1           | 15         | 23       |
    | 3          | 2           | 10         | 12       |
    | 4          | 2           | 17         | 28       |
    | 5          | 2           | 2          | 8        |

    Ads table:

    | ad_id | customer_id | timestamp |
    | ----- | ----------- | --------- |
    | 1     | 1           | 5         |
    | 2     | 2           | 17        |
    | 3     | 2           | 20        |

    Result table:

    | session_id |
    | ---------- |
    | 2          |
    | 3          |
    | 5          |

    The ad with ID 1 was shown to user 1 at time 5 while they were in session 1.
    The ad with ID 2 was shown to user 2 at time 17 while they were in session 4.
    The ad with ID 3 was shown to user 2 at time 20 while they were in session 4.

    We can see that sessions 1 and 4 had at least one ad. Sessions 2, 3, and 5 did not have any ads, so we return them.

    ```SQL
    SELECT DISTINCT session_id FROM playback 
    NATURAL JOIN ads
    WHERE timestamp < start_time
    OR timestamp > end_time
    ORDER BY session_id
    ```

29. There is a table courses with columns: student and class

    Please list out all classes which have more than or equal to 5 students.

    For example, the table:

    | student | class    |
    | ------- | -------- |
    | A       | Math     |
    | B       | English  |
    | C       | Math     |
    | D       | Biology  |
    | E       | Math     |
    | F       | Computer |
    | G       | Math     |
    | H       | Math     |
    | I       | Math     |

    Should output:

    | class |
    | ----- |
    | Math  |

    ```SQL
    SELECT class FROM courses
    GROUP BY class
    HAVING count(*)>=5
    ```

30. Table point holds the x coordinate of some points on x-axis in a plane, which are all integers.

    Write a query to find the shortest distance between two points in these points.

    | x   |
    |-----|
    | -1  |
    | 0   |
    | 2   |

    The shortest distance is '1' obviously, which is from point '-1' to '0'. So the output is as below:

    | shortest|
    |---------|
    |  1      |

    ```SQL
    SELECT MIN(ABS(p1.x - p2.x)) AS shortest
    FROM point p1
    CROSS JOIN point p2
    WHERE p1.x <> p2.x;
    ```

31. Suppose that a website contains two tables, the Customers table and the Orders table. Write a SQL query to find all customers who never order anything.

    Table: Customers.

    | Id | NameCust |
    |----|----------|
    | 1  | Joe      |
    | 2  | Henry    |
    | 3  | Sam      |
    | 4  | Max      |

    Table: Orders.

    | Id | CustomerId |
    |----|------------|
    | 1  | 3          |
    | 2  | 1          |

    Using the above tables as example, return the following:

    | Customers |
    |-----------|
    | Henry     |
    | Max       |

    ```SQL
    SELECT namecust AS "Customers" FROM Customers
    LEFT JOIN Orders
    ON Customers.Id = Orders.CustomerId
    WHERE Orders.CustomerId IS NULL
    ```

32. Write a SQL query to delete all duplicate email entries in a table named Person, keeping only unique emails based on its smallest Id.

    | Id | Email            |
    |----|------------------|
    | 1  | johnexample.com |
    | 2  | bobexample.com  |
    | 3  | johnexample.com |

    Id is the primary key column for this table.
    For example, after running your query, the above Person table should have the following rows:

    | Id | Email            |
    |----|------------------|
    | 1  | johnexample.com |
    | 2  | bobexample.com  |

    Your output is the whole Person table after executing your sql. Use delete statement.

    ```SQL
    DELETE FROM Person
    WHERE id NOT IN(
        SELECT
        MIN(id) OVER(PARTITION BY email)
        FROM person
    );

    SELECT * FROM Person;
    ```

33. Employees Earning More Than Their Managers
 The Employee table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

    | Id | Name  | Salary | ManagerId |
    |----|-------|--------|-----------|
    | 1  | Joe   | 70000  | 3         |
    | 2  | Henry | 80000  | 4         |
    | 3  | Sam   | 60000  | NULL      |
    | 4  | Max   | 90000  | NULL      |

    Given the Employee table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager.

    | Employee |
    |----------|
    | Joe      |

    ```SQL
    SELECT E.Name AS Employee FROM Employee E
    JOIN Employee M
    ON E.Id=M.ManagerID
    WHERE E.Salary > M.Salary
    ```

34. Write an SQL query to find each query_name, the quality.

    Table: Queries

    | Column Name | Type    |
    |-------------|---------|
    | query_name  | varchar |
    | result      | varchar |
    | position    | int     |
    | rating      | int     |

    There is no primary key for this table, it may have duplicate rows.
    This table contains information collected from some queries on a database.
    The position column has a value from 1 to 500.
    The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.

    We define query quality as:

    The average of the ratio between query rating and its position.

    Quality should be rounded to 2 decimal places.

    The query result format is in the following example:

    Queries table:

    | query_name | result           | position | rating |
    |------------|------------------|----------|--------|
    | Dog        | Golden Retriever | 1        | 5      |
    | Dog        | German Shepherd  | 2        | 5      |
    | Dog        | Mule             | 200      | 1      |
    | Cat        | Shirazi          | 5        | 2      |
    | Cat        | Siamese          | 3        | 3      |
    | Cat        | Sphynx           | 7        | 4      |

    Result table:

    | query_name | quality |
    |------------|---------|
    | Dog        | 2.50    |
    | Cat        | 0.66    |

    Dog queries quality is ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50

    Cat queries quality equals ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66

    ```SQL
    SELECT query_name,
    ROUND(AVG((rating * 1.0) / position), 2) AS quality
    FROM Queries
    GROUP BY query_name;
    ```

35. Write an SQL query to find the total score for each gender at each day. Order the result table by gender and dayTable: Scores

    | Column Name  | Type    |
    |-------------- |---------|
    | player_name  | varchar |
    | gender       | varchar |
    | day          | date    |
    | score_points | int     |

    (gender, day) is the primary key for this table.
    A competition is held between females team and males team.
    Each row of this table indicates that a player_name and with gender has scored score_point in someday.
    Gender is 'F' if the player is in females team and 'M' if the player is in males team.

    The query result format is in the following example:

    Scores table:

    | player_name | gender | day        | score_points |
    |------------ |--------|------------|--------------|
    | Aron        | F      | 2020-01-01 | 17           |
    | Alice       | F      | 2020-01-07 | 23           |
    | Bajrang     | M      | 2020-01-07 | 7            |
    | Khali       | M      | 2019-12-25 | 11           |
    | Slaman      | M      | 2019-12-30 | 13           |
    | Joe         | M      | 2019-12-31 | 3            |
    | Jose        | M      | 2019-12-18 | 2            |
    | Priya       | F      | 2019-12-31 | 23           |
    | Priyanka    | F      | 2019-12-30 | 17           |

    Result table:

    | gender | day        | total |
    |-------- |------------|-------|
    | F      | 2019-12-30 | 17    |
    | F      | 2019-12-31 | 40    |
    | F      | 2020-01-01 | 57    |
    | F      | 2020-01-07 | 80    |
    | M      | 2019-12-18 | 2     |
    | M      | 2019-12-25 | 13    |
    | M      | 2019-12-30 | 26    |
    | M      | 2019-12-31 | 29    |
    | M      | 2020-01-07 | 36    |

    For females team:
    First day is 2019-12-30, Priyanka scored 17 points and the total score for the team is 17.
    Second day is 2019-12-31, Priya scored 23 points and the total score for the team is 40.
    Third day is 2020-01-01, Aron scored 17 points and the total score for the team is 57.
    Fourth day is 2020-01-07, Alice scored 23 points and the total score for the team is 80.
    For males team:
    First day is 2019-12-18, Jose scored 2 points and the total score for the team is 2.
    Second day is 2019-12-25, Khali scored 11 points and the total score for the team is 13.
    Third day is 2019-12-30, Slaman scored 13 points and the total score for the team is 26.
    Fourth day is 2019-12-31, Joe scored 3 points and the total score for the team is 29.
    Fifth day is 2020-01-07, Bajrang scored 7 points and the total score for the team is 36.

    ```SQL
    SELECT gender, day, 
    SUM(score_points) OVER(PARTITION BY gender ORDER BY day) as total
    FROM Scores
    ORDER BY gender, day;
    ```
