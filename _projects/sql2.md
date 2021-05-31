# Medium 

[[177]](#question-177-nth-highest-salary)
[[534]](#question-534-game-play-analysis-3)
[[550]](#question-550-game-play-analysis-4)
[[570]](#question-570-managers-with-at-least-5-direct-reports)
[[574]](#question-574-winning-candidate)
[[578]](#question-578-get-highest-answer-rate-question)
[[580]](#question-580-count-student-number-in-departments)
[[585]](#question-585-investments-in-2016)
[[602]](#question-602-friend-requests-2)
[[608]](#question-608-tree-node)
[[612]](#question-612-shortest-distance-in-a-plane)
[[614]](#question-614-second-degree-follower)
[[626] :confused:](#question-626-exchange-seats)
[[1045]](#question-1045-customers-who-bought-all-products)
[[1070]](#question-1070-products-sales-analysis-3)
[[1077]](#question-1077-project-employees-3)
[[1098]](#question-1098-unpopular-books)  
[[1107]](#question-1107-new-users-daily-count) 
[[1112]](#question-1112-highest-grade-for-each-student) 
[[1126]](#question-1112-active-businesses) 
[[1132]](#question-1132-reported-posts-2)
[[1149]](#question-1149-article-views-2)
[[1158]](#question-1158-market-analysis-1)
[[1164] :confused:](#question-1164-product-price-at-a-given-date)
[[1193]](#question-1193-monthly-transactions-1)
[[1204]](#question-1204-last-person-to-fit-in-the-elevator)
[[1205]](#question-1205-monthly-transactions-2)
[[1212]](#question-1212-team-scores-in-football-tournament)
[[1264]](#question-1264-page-recommendations)
[[1270]](#question-1264-all-people-given-to-the-manager) 

-----------------------------------------------------------------------------

# Question 177: Nth highest salary

Write a SQL query to get the nth highest salary from the Employee table.

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

| getNthHighestSalary(2) |
| 200                    |

``` sql 
-- Create function
CREATE FUNCTION nthHighestSalary(
        n int
)
RETURNs DECIMAL(10, 2)
DETERMINISTIC
BEGIN 
    DECLARE salary DECIMAL(10, 2); 
   
    WITH t as 
    (SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as ranks)
    SELECT IFNULL(DISTINCT salary, 0) as salary
    FROM t 
    WHERE ranks = n; 
    
    RETURN salary; 
END;
```

# Question 178: Rank scores

Write a SQL query to rank scores.
If there is a tie between two scores, both should have the same ranking. 
Note that after a tie, the next ranking number should be the next consecutive integer value. 
In other words, there should be no "holes" between ranks.

| Id | Score |
|----|-------|
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |

For example, given the above Scores table, your query should generate the following report (order by highest score):

| score | Rank    |
|-------|---------|
| 4.00  | 1       |
| 4.00  | 1       |
| 3.85  | 2       |
| 3.65  | 3       |
| 3.65  | 3       |
| 3.50  | 4       |

``` sql 
-- RANK()/DENSE_RANK()
SEELCT score, 
       DENSE_RANK() OVER (score DESC) as ranks 
FROM scores; 
```

# Question 180: Consecutive Numbers

Write a SQL query to find all numbers that appear at least three times consecutively.

| Id | Num |
|----|-----|
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |

For example, given the above Logs table, 1 is the only number that appears consecutively for at least three times.

| ConsecutiveNums |
|-----------------|
| 1               |
|-----------------|

``` sql 
-- self join
SELECT DISTINCT num as ConsecutiveNums
FROM numbers as t1 JOIN numbers as t2 
     ON t1.num = t2.num AND t1.id = t2.id - 1 
     JOIN numbers as t3 
     ON t2.num = t3.num AND t2.id = t3.id - 1; 

-- window function
```

# Question 184: Department Highest Salary

The Employee table holds all employees. Every employee has an Id, a salary, and there is also a column for the department Id.

| Id | Name  | Salary | DepartmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |

The Department table holds all departments of the company.

| Id | Name     |
|----|----------|
| 1  | IT       |
| 2  | Sales    |

Write a SQL query to find employees who have the highest salary in each of the departments. 

For the above tables, your SQL query should return the following rows (order of rows does not matter).

|------------|----------|--------|
| Department | Employee | Salary |
|------------|----------|--------|  
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |

Explanation:

Max and Jim both have the highest salary in the IT department and Henry has the highest salary in the Sales department.

``` sql 
-- RANK() function
WITH rank_salary as (
SELECT d.name as Department, e.name as Employee, e.salary as Salary, 
       RANK() OVER (d.name ORDER BY e.salary DESC) as salary_rank
FROM employees as e JOIN departments as d 
     ON e.departmentid = d.id
) 
SEELCT Department, Employee, Salary
FROM rank_salary
WHERE salary_rank = 1; 
```
# Question 534: Game Play Analysis 3 

Write an SQL query that reports for each player and date, how many games played so far by the player. That is, the total number of games played by the player until that date. Check the example for clarity.

The query result format is in the following example:

Activity table:

| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 1         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

Result table:

| player_id | event_date | games_played_so_far |
|-----------|------------|---------------------|
| 1         | 2016-03-01 | 5                   |
| 1         | 2016-05-02 | 11                  |
| 1         | 2017-06-25 | 12                  |
| 3         | 2016-03-02 | 0                   |
| 3         | 2018-07-03 | 5                   |

For the player with id 1, 5 + 6 = 11 games played by 2016-05-02, and 5 + 6 + 1 = 12 games played by 2017-06-25.

For the player with id 3, 0 + 5 = 5 games played by 2018-07-03.

Note that for each player we only care about the days when the player logged in.

``` sql 
-- cumulative sum/running total using partition
SELECT player_id, 
       event_date, 
       SUM(games_played) OVER (PARTITION BY player_id ORDER BY event_date) as games_played_so_far 
FROM activity
ORDER BY 1, 2;   
```

# Question 	550: Game Play Analysis 4

Write an SQL query that reports the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. 

In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

``` sql 
-- self join 
WITH t as (
SELECT player_id, min(event_date) as start_date 
FROM avtivity
GROUP BY player_id) 
SELECT ROUND(COUNT(a.event_date)/COUNT(start_date), 2) as fraction 
FROM t LEFT JOIN activity as a 
     ON DATEDIFF(t.start_date, a.event_date) = 1 
     AND t.player_id = a.player_id; 
```

# Question 570: Managers with at Least 5 Direct Reports

The Employee table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

Given the Employee table, write a SQL query that finds out managers with at least 5 direct report. 

``` sql
-- inner join 
WITH t as (
SELECT e1.id as id, e2.name as name, e2.id as managers 
FROM employee as e1 JOIN employee as e2 
     ON e1.managerid = e2.id)
SELECT name
FROM t 
GROUP BY managers 
HAVING COUNT(DIStinct id) >= 5; 

-- subquery 
-- more efficient if many records
SELECT name 
FROM employee 
WHERE ID IN (
    SELECT managerid
    FROM employee 
    GROUPY managerid
    HAVING COUNT(managerid) > 5);  
```

# Question 574: Winning Candidate

Table: Candidate

| id  | Name    |
|-----|---------|
| 1   | A       |
| 2   | B       |
| 3   | C       |
| 4   | D       |
| 5   | E       |
 
Table: Vote

| id  | CandidateId  |
|-----|--------------|
| 1   |     2        |
| 2   |     4        |
| 3   |     3        |
| 4   |     2        |
| 5   |     5        |

id is the auto-increment primary key, CandidateId is the id appeared in Candidate table.

Write a sql to find the name of the winning candidate, the above example will return the winner B.

``` sql 
-- inner/left join
WITH t as (
SELECT c.id as id, c.name as name, COUNT(v.candidateid) as total_vote  
FROM candidate as c LEFT JOIN vote as v 
     ON c.id = v.candidateid
GROUP BY c.id
ORDER BY 3) 
SELECT name
FROM t
LIMIT 1;
```

# Question 578: Get Highest Answer Rate Question

Get the highest answer rate question from a table survey_log with these columns: id, action, question_id, answer_id, q_num, timestamp.

id means user id; action has these kind of values: "show", "answer", "skip"; answer_id is not null when action column is "answer", while is null for "show" and "skip"; q_num is the numeral order of the question in current session.

Write a sql query to identify the question which has the highest answer rate.

``` sql 

```

# Question 580: Count student number in departments

A university uses 2 data tables, student and department, to store data about its students and the departments associated with each major.

Write a query to print the respective department name and number of students majoring in each department for all departments in the department table (even ones with no current students).

Sort your results by descending number of students; if two or more departments have the same number of students, then sort those departments alphabetically by department name.

The student is described as follow:

| Column Name  | Type      |
|--------------|-----------|
| student_id   | Integer   |
| student_name | String    |
| gender       | Character |
| dept_id      | Integer   |

where student_id is the student's ID number, student_name is the student's name, gender is their gender, and dept_id is the department ID associated with their declared major.

And the department table is described as below:

| Column Name | Type    |
|-------------|---------|
| dept_id     | Integer |
| dept_name   | String  |

where dept_id is the department's ID number and dept_name is the department name.

``` sql 
SELECT d.dept_name as dept_name, COUNT(DISTINCT student_id) as student_number
FROM department as d LEFT JOIN student as s 
     ON d.dept_id = s.dept_id 
GROUP BY d.dept_id
ORDER BY 2 DESC, 1; 
```

# Question 585: Investments in 2016

Write a query to print the sum of all total investment values in 2016 (TIV_2016), to a scale of 2 decimal places, for all policy holders who meet the following criteria:

Have the same TIV_2015 value as one or more other policyholders.

Are not located in the same city as any other policyholder (i.e.: the (latitude, longitude) attribute pairs must be unique).

Input Format:

The insurance table is described as follows:

| Column Name | Type          |
|-------------|---------------|
| PID         | INTEGER(11)   |
| TIV_2015    | NUMERIC(15,2) |
| TIV_2016    | NUMERIC(15,2) |
| LAT         | NUMERIC(5,2)  |
| LON         | NUMERIC(5,2)  |

where PID is the policyholder's policy ID, TIV_2015 is the total investment value in 2015, TIV_2016 is the total investment value in 2016, LAT is the latitude of the policy holder's city, and LON is the longitude of the policy holder's city.

Sample Input

| PID | TIV_2015 | TIV_2016 | LAT | LON |
|-----|----------|----------|-----|-----|
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |

Sample Output

| TIV_2016 |
|----------|
| 45.00    |

Explanation

The first record in the table, like the last record, meets both of the two criteria.

The TIV_2015 value '10' is as the same as the third and forth record, and its location unique.

The second record does not meet any of the two criteria. Its TIV_2015 is not like any other policyholders.

And its location is the same with the third record, which makes the third record fail, too.

So, the result is the sum of TIV_2016 of the first and last record, which is 45.

``` sql 
-- equi-join
WITH t as (SELECT lat, lon 
FROM insurance
GROUP BY lat, lon  
HAVING COUNT(*) = 1)

SELECT SUM(i.TIV_2016)
FROM insurance as i JOIN t 
     ON i.lat = t.lat AND i.lon = t.lon
GROUP BY i.TIV_2015 
HAVING COUNT(*) > 1; 
```

# Question 602: Friend Requests 2 

| requester_id | accepter_id | accept_date|
|--------------|-------------|------------|
| 1            | 2           | 2016_06-03 |
| 1            | 3           | 2016-06-08 |
| 2            | 3           | 2016-06-08 |
| 3            | 4           | 2016-06-09 |

| requester_id | accepter_id | accept_date|
|--------------|-------------|------------|
| 1            | 2           | 2016_06-03 |
| 1            | 3           | 2016-06-08 |
| 2            | 3           | 2016-06-08 |
| 3            | 4           | 2016-06-09 |

``` sql
-- UNION 
WITH t1 as (
SELECT requester_id as id, accepter_id as friend, accept_date
FROM request_accepted 
UNION ALL
SELECT accepter_id as id, requester_id as friend, accept_date
FROM request_accepted), 
t2 as (
SELECT id, COUNT(DISTINCT friend) as num_friends
FROM t 
GROUP BY id)
SELECT id, MAX(num_friends) FROM t2; 

-- OUTER JOIN ???
SELECT COALESCE(t1.requester_id, t2.accepter_id) as 
FROM request_accepted as t1 OUTER JOIN request_accepted as t2 
     ON t1.accepter_id = t2.requester_id 
``` 

# Question 608: Tree Node 

Given a table tree, id is identifier of the tree node and p_id is its parent node's id.

| id | p_id |
|----|------|
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |

Each node in the tree can be one of three types:

Leaf: if the node is a leaf node.

Root: if the node is the root of the tree.

Inner: If the node is neither a leaf node nor a root node.
 
Write a query to print the node id and the type of the node. Sort your output by the node id. The result for the above sample is:
 
| id | Type |
|----|------|
| 1  | Root |
| 2  | Inner|
| 3  | Leaf |
| 4  | Leaf |
| 5  | Leaf |

``` sql 
WITH RECURSIVE t1 (id, p_id, level)
as (
    SELECT id, p_id, 0  
    FROM tree
    WHERE p_id IS NULL
    
    UNION ALL 
    
    SELECT tree.id, tree.p_id, t.level + 1    
    FROM tree, t 
    WHERE tree.p_id = t.id 
)
SELECT id,
       CASE WHEN level = (SEELCT MAX(level) from t1) THEN 'Leaf' 
            WHEN level = (SEELCT MIN(level) from t1) THEN 'Root'
            ELSE 'Inner'
       END as type 
FROM t1 
```

# Question 612: Shortest distance in a plane

Table point_2d holds the coordinates (x,y) of some unique points (more than two) in a plane.
 
Write a query to find the shortest distance between these points rounded to 2 decimals.
 

| x  | y  |
|----|----|
| -1 | -1 |
| 0  | 0  |
| -1 | -2 |
 
The shortest distance is 1.00 from point (-1,-1) to (-1,2). So the output should be:

| shortest |
|----------|
| 1.00     |

``` sql
SELECT ROUND(MIN(distances), 2) AS shortest
FROM 
    (SELECT SQRT(POWER((p1.x - p2.x), 2) + POWER((p1.y - p2.y), 2)) as distances 
    FROM point as p1 CROSS JOIN point as p1
    WHERE p1.x <> p2.x or p1.y <> p2.y); 
```

# Question 614: Second degree follower


In facebook, there is a follow table with two columns: followee, follower.

Please write a sql query to get the amount of each follower’s follower if he/she has one.

For example:

| followee    | follower   |
|-------------|------------|
|     A       |     B      |
|     B       |     C      |
|     B       |     D      |
|     D       |     E      |

should output:

| follower    | num        |
|-------------|------------|
|     B       |  2         |
|     D       |  1         |

Explaination:
 
Both B and D exist in the follower list, when as a followee, B's follower is C and D, and D's follower is E. A does not exist in follower list.

``` sql 
--subquery
SELECT followee as follower, COUNT(follower) as num
FROM table 
WHERE followee IN (
    SELECT DISTINCT follower  
    FROM table)
GROUP BY followee 

-- join 
SELECT t1.follower as follower, COUNT(t2.followee) as num 
FROM table t1 JOIN table t2 
     ON t1.follower = t2.followee 
GROUP BY t1.follower 
```

# Question 626: Exchange Seats 

Mary is a teacher in a middle school and she has a table seat storing students' names and their corresponding seat ids.

The column id is continuous increment.
 
Mary wants to change seats for the adjacent students.
 
Can you write a SQL query to output the result for Mary?

|    id   | student |
|---------+---------|
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |

For the sample input, the output is:

|    id   | student |
|---------|---------|
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |

``` sql 

```

# Question 1045: Customers Who Bought All Products

Table: Customer

| Column Name | Type    |
|-------------|---------|
| customer_id | int     |
| product_key | int     |

product_key is a foreign key to Product table.

Table: Product

| Column Name | Type    |
|-------------|---------|
| product_key | int     |

product_key is the primary key column for this table.
 
Write an SQL query for a report that provides the customer ids from the Customer table that bought all the products in the Product table.

``` sql 
--subquery 
SELECT customer_id
FROM customer 
GROUP BY customer_id 
HAVING COUNT(DISTINCT product_key) = (
                SELECT COUNT(product_id)
                FROM product); 
```

# Question 1070: Product Sales Analysis 3

Table: Sales

| Column Name | Type  |
|-------------|-------|
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

sale_id is the primary key of this table.

product_id is a foreign key to Product table.
Note that the price is per unit.

Table: Product

| Column Name  | Type    |
|--------------|---------|
| product_id   | int     |
| product_name | varchar |

product_id is the primary key of this table.

Write an SQL query that selects the product id, year, quantity, and price for the first year of every product sold.

``` sql 
-- RANK() 
WITH t as 
(
SELECT product_id, quantity, price, year
       RANK() OVER (PARTITION BY product_id ORDER BY year ASC) as year_rank 
FROM sales
)
SELECT product_id, quantity, price, year
FROM t 
WHERE year_rank = 1 -- first year for each product
```

# Question 1077: Project employees 3 

Table: Project

| Column Name | Type    |
|-------------|---------|
| project_id  | int     |
| employee_id | int     |

(project_id, employee_id) is the primary key of this table.

employee_id is a foreign key to Employee table.

Table: Employee

| Column Name      | Type    |
|------------------|---------|
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |

employee_id is the primary key of this table.
 
Write an SQL query that reports the most experienced employees in each project. 

In case of a tie, report all employees with the maximum number of experience years.

``` sql 
-- rank() function
WITH t as 
(
SELECT p.project_id as project_id, p.employee_id as employee_id, 
       e.experience_years as experience_years,
       RANK() OVER (PARTITION BY p.project_id ORDER BY e.experience_years DESC) as rank_experiences
FROM project as p JOIN employee as e 
     ON p.employee_id = e.employee_id
)
SELECT 
FROM t
WHERE rank_experiences = 1; 
```

# Question 1098: Unpopular Books

Table: Books

| Column Name    | Type    |
|----------------|---------|
| book_id        | int     |
| name           | varchar |
| available_from | date    |

book_id is the primary key of this table.

Table: Orders

| Column Name    | Type    |
|----------------|---------|
| order_id       | int     |
| book_id        | int     |
| quantity       | int     |
| dispatch_date  | date    |

order_id is the primary key of this table.

book_id is a foreign key to the Books table.
 
Write an SQL query that reports the books that have sold less than 10 copies in the last year, excluding books that have been available for less than 1 month from today. Assume today is 2019-06-23.

``` sql 
-- subquery 
SELECT b.book_id, b.book_name
FROM
    (SELECT book_id, quantity  
    FROM orders 
    WHERE dispath_date > '2018-06-23' and dispath_date < 2019-06-23) as t
    RIGHT JOIN books as b 
    ON t.book_id = b.book_id 
WHERE available_from > '2019-05-23'
GROUP BY b.book_id, b.book_name
HAVING COALESCE(SUM(t.quantity), 0) < 10; 
```

# Question 1107: New users daily count

Table: Traffic

| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| activity      | enum    |
| activity_date | date    |

There is no primary key for this table, it may have duplicate rows.

The activity column is an ENUM type of ('login', 'logout', 'jobs', 'groups', 'homepage').
 
Write an SQL query that reports for every date within at most 90 days from today, the number of users that logged in for the first time on that date. Assume today is 2019-06-30.

``` sql
-- rank()
WITH t as 
(SELECT user_id, 
       activity,
       activity_date 
       RANK() OVER (PARTITION BY user_id ORDER BY activity_date) as rank_login_date
       -- this ensures us to find the first login day for each user
FROM Traffic 
WHERE activity = 'login'
) as t 
SELECT activity_date as date, COOUNT(user_id) as num_of_users  
FROM t
WHEREE rank_login_date = 1 --find first login date for each user
       AND activity_date < '2019-06-30' AND activity_date > '2019-04-01' 
GROUP BY activity_date; 
```

# Question 1112:  Highest Grade For Each Student 

Table: Enrollments

| Column Name   | Type    |
|---------------|---------|
| student_id    | int     |
| course_id     | int     |
| grade         | int     |

(student_id, course_id) is the primary key of this table.

Write a SQL query to find the highest grade with its corresponding course for each student. In case of a tie, you should find the course with the smallest course_id. The output must be sorted by increasing student_id.

``` sql 
SELECT student_id, course_id, MAX(grade)
FROM enrollments
GROUP BY student_id, course_id
ORDER BY 1;  -- this can return multiple rows for a student, which valids the requirements. 
-- use rank 
WITH t as (
SELECT student_id, course_id, grade
       RANK() OVER (PARTITION BY student_id ORDER BY grade DESC, course_id) as ranks 
FROM enrollments)
SELECT student_id, course_id, grade 
FROM t 
WHERE ranks = 1; 
```

# Question 1126: Active Businesses

Table: Events

| Column Name   | Type    |
|---------------|---------|
| business_id   | int     |
| event_type    | varchar |
| occurences    | int     | 

(business_id, event_type) is the primary key of this table.

Each row in the table logs the info that an event of some type occured at some business for a number of times.

Write an SQL query to find all active businesses.

An active business is a business that has more than one event type with occurences greater than the average occurences of that event type among all businesses.

``` sql 
--easy, inner join
```

# Question 1132: Reported Posts 2 

Table: Actions  

| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| post_id       | int     |
| action_date   | date    |
| action        | enum    |
| extra         | varchar |

There is no primary key for this table, it may have duplicate rows.

The action column is an ENUM type of ('view', 'like', 'reaction', 'comment', 'report', 'share').

The extra column has optional information about the action such as a reason for report or a type of reaction. 

Table: Removals

| Column Name   | Type    |
|---------------|---------|
| post_id       | int     |
| remove_date   | date    | 

post_id is the primary key of this table.

Each row in this table indicates that some post was removed as a result of being reported or as a result of an admin review.

Write an SQL query to find the average for daily percentage of posts that got removed after being reported as spam, rounded to 2 decimal places.

``` sql 

```

# Question 1149: Article Views 2 

Table: Views

| Column Name   | Type    |
|---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |

There is no primary key for this table, it may have duplicate rows.

Each row of this table indicates that some viewer viewed an article (written by some author) on some date. 

Note that equal author_id and viewer_id indicate the same person.
 
Write an SQL query to find all the people who viewed more than one article on the same date, sorted in ascending order by their id.

``` sql 
SELECT view_id 
FROM views 
GROUP BY viewer_id, view_date 
HAVING COUNT(DISTINCT article_id) > 1
ORDER BY 1; 
```

# Question 1158: Market Analysis 1 

Table: Users

| Column Name    | Type    |
|----------------|---------|
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |

user_id is the primary key of this table.

This table has the info of the users of an online shopping website where users can sell and buy items.

Table: Orders

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | order_id      | int     |
-- | order_date    | date    |
-- | item_id       | int     |
-- | buyer_id      | int     |
-- | seller_id     | int     |
-- +---------------+---------+
-- order_id is the primary key of this table.
-- item_id is a foreign key to the Items table.
-- buyer_id and seller_id are foreign keys to the Users table.
-- Table: Items

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | item_id       | int     |
-- | item_brand    | varchar |

item_id is the primary key of this table.
 
Write an SQL query to find for each user, the join date and the number of orders they made as a buyer in 2019.

``` sql 
SELECT u.user_id, u.join_date, COUNT(o.order_id) as num_orders_2019
FROM users as u LEFT JOIN orders as o 
     ON u.user_id = o.buyer_id
WHERE YEAR(o.order_date) = 2019
GROUP BY u.user_id, u.join_date
ORDER BY 1; 

-- more effcient
SELECT users.user_id, users.join_date, COALESCE(COUNT(t.order_id), 0) as counts 
FROM users LEFT JOIN 
         (SELECT order_id, order_date, buyer_id  
         FROM orders 
         WHERE YEAR(order_date) = 2019) AS t 
     ON users.user_id = t.buyer_id 
GROUP BY users.user_id
ORDER BY 1; 
```

# Question 1164: Product Price at a given date 

Table: Products

| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |

(product_id, change_date) is the primary key of this table.

Each row of this table indicates that the price of some product was changed to a new price at some date.
 
Write an SQL query to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

``` sql

```

# Question 1174: Immediate Food Delivery 2 

Table: Delivery

| Column Name                 | Type    |
|-----------------------------|---------|
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |

delivery_id is the primary key of this table.

The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).

The first order of a customer is the order with the earliest order date that customer made. It is guaranteed that a customer has exactly one first order.

Write an SQL query to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

The query result format is in the following example:

If the preferred delivery date of the customer is the same as the order date then the order is called immediate otherwise it's called scheduled.

The first order of a customer is the order with the earliest order date that customer made. It is guaranteed that a customer has exactly one first order.

Write an SQL query to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

``` sql
WITH t as 
(SELECT delivery_id, customer_id, order_date, customer_pref_delivery_date
        RANK() OVER (PARTITION BY customer_id ORDER BY order_date) as dates 
FROM delivery
)
SELECT AVG((CASE WHEN order_date = customer_pref_delivery_date 
            THEN 1 ELSE O END)) as immediate_percentage
FROM t
WHERE dates = 1
```

# Question 1193: Monthly Transactions 1 

-- Table: Transactions

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | id            | int     |
-- | country       | varchar |
-- | state         | enum    |
-- | amount        | int     |
-- | trans_date    | date    |
-- +---------------+---------+
-- id is the primary key of this table.
-- The table has information about incoming transactions.
-- The state column is an enum of type ["approved", "declined"].
 

-- Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

``` sql 
- CASE WHEN 
SELECT EXTRACT(MONTH FROM trans_date) as month, 
       country, 
       COUNT(id) as number of transactions,
       SUM(amount) as total_amount, 
       COUNT(CASE WHEN state = "approved" THEN 1 ELSE 0 END) as number_approved_transactions,
       SUM(CASE WHEN state = "approved" THEN amount ELSE 0 END) as approved_total_amount
FROM Transactions
GROUP BY EXTRACT(MONTH FROM trans_date), country
ORDER BY 1, 2;
```
# Question 1204: Last Person to Fit in the Elevator 

-- Table: Queue

-- +-------------+---------+
-- | Column Name | Type    |
-- +-------------+---------+
-- | person_id   | int     |
-- | person_name | varchar |
-- | weight      | int     |
-- | turn        | int     |
-- +-------------+---------+
-- person_id is the primary key column for this table.
-- This table has the information about all people waiting for an elevator.
-- The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.
 

-- The maximum weight the elevator can hold is 1000.

-- Write an SQL query to find the person_name of the last person who will fit in the elevator without exceeding the weight limit. It is guaranteed that the person who is first in the queue can fit in the elevator.

``` sql 
WITH t as 
(
SELECT person_id, person_name, weight, turn,
       SUM(weight) OVER (order by turn) as cum_sum  
FROM queue
ORDER BY turn DESC
)
SELECT person_name   
FROM t 
WHERE cum_sum < 1000
LIMIT 1; 
```

# Question 1205: Monthly Transactions 2

-- Table: Transactions

-- +----------------+---------+
-- | Column Name    | Type    |
-- +----------------+---------+
-- | id             | int     |
-- | country        | varchar |
-- | state          | enum    |
-- | amount         | int     |
-- | trans_date     | date    |
-- +----------------+---------+
-- id is the primary key of this table.
-- The table has information about incoming transactions.
-- The state column is an enum of type ["approved", "declined"].
-- Table: Chargebacks

-- +----------------+---------+
-- | Column Name    | Type    |
-- +----------------+---------+
-- | trans_id       | int     |
-- | charge_date    | date    |
-- +----------------+---------+
-- Chargebacks contains basic information regarding incoming chargebacks from some transactions placed in Transactions table.
-- trans_id is a foreign key to the id column of Transactions table.
-- Each chargeback corresponds to a transaction made previously even if they were not approved.
 

-- Write an SQL query to find for each month and country, the number of approved transactions and their total amount, the number of chargebacks and their total amount.

``` sql 
-- inner join, union
WITH t as (id, country, state, amount, trans_date)
(
SELECT * from Transactions
UNION ALL 
SELECT t.id as id, t.country as country, NULL as state, t.amount as amount, c.trans_date -- fill table chargeback with amount and country info 
FROM Transactions as t JOIN Chargebacks as c 
     ON t.id = c.trans_id 
)
SELECT EXTRACT(MONTH FROM trans_date), country,
       COUNT(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) as approved_count,
       SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) as approved_amount, 
       COUNT(CASE WHEN state IS NULL THEN 1 ELSE O END) as chargeback_count,
       SUM(CASE WHEN state IS NULL THEN chargeback_amount ELSE 0 END) as chargeback_amount -- can be better by add one more columns to specify transaction or chargeback 
FROM t 
GROUPY EXTRACT(MONTH FROM trans_date), country
ORDER BY 1, 2; 
```

# Question 1212: team scores in football tournament

Table: Teams

| Column Name   | Type     |
|---------------|----------|
| team_id       | int      |
| team_name     | varchar  |

team_id is the primary key of this table.

Each row of this table represents a single football team.

Table: Matches

| Column Name   | Type    |
|---------------|---------|
| match_id      | int     |
| host_team     | int     |
| guest_team    | int     | 
| host_goals    | int     |
| guest_goals   | int     |

match_id is the primary key of this table.

Each row is a record of a finished match between two different teams. 

Teams host_team and guest_team are represented by their IDs in the teams table (team_id) and they scored host_goals and guest_goals goals respectively.
 
You would like to compute the scores of all teams after all matches. Points are awarded as follows:
A team receives three points if they win a match (Score strictly more goals than the opponent team).

A team receives one point if they draw a match (Same number of goals as the opponent team).

A team receives no points if they lose a match (Score less goals than the opponent team).

Write an SQL query that selects the team_id, team_name and num_points of each team in the tournament after all described matches. Result table should be ordered by num_points (decreasing order). In case of a tie, order the records by team_id (increasing order).

The query result format is in the following example:

Teams table:

| team_id   | team_name    |
|-----------|--------------|
| 10        | Leetcode FC  |
| 20        | NewYork FC   |
| 30        | Atlanta FC   |
| 40        | Chicago FC   |
| 50        | Toronto FC   |


Matches table:

| match_id   | host_team    | guest_team    | host_goals  | guest_goals  |
|------------|--------------|---------------|-------------|--------------|
| 1          | 10           | 20            | 3           | 0            |
| 2          | 30           | 10            | 2           | 2            |
| 3          | 10           | 50            | 5           | 1            |
| 4          | 20           | 30            | 1           | 0            |
| 5          | 50           | 30            | 1           | 0            |

Result table:

| team_id    | team_name    | num_points    |
|------------|--------------|---------------|
| 10         | Leetcode FC  | 7             |
| 20         | NewYork FC   | 3             |
| 50         | Toronto FC   | 3             |
| 30         | Atlanta FC   | 1             |
| 40         | Chicago FC   | 0             |

``` sql 
WITH all_games_for_each_team (team_id, team_goals, other_goals) as
(
SELECT host_team, host_goals, guest_goals FROM matches
UNION ALL 
SELECT guest_team, guest_goals, host_goals FROM matches
)
SELECT t.team_id as team_id, team.team_name as team_name, t.num_points
FROM 
    (SELECT all_games_for_each_team.team_id as team_id,
           SUM(CASE WHEN team_goals > other_goals THEN 3
                    WHEN team_goals = other_goals THEN 1
                    ELSE 0 
               END) as num_points
    FROM all_games_for_each_team
    GROUP BY team_id) AS t 
    JOIN team ON t.team_id = team.team_id; 
-- id and team_name, how to group by?? 
```

# question 1264: page recommendations
   
Table: Friendship

| Column Name   | Type    |
|---------------|---------|
| user1_id      | int     |
| user2_id      | int     |

(user1_id, user2_id) is the primary key for this table.

Each row of this table indicates that there is a friendship relation between user1_id and user2_id.
 
Table: Likes

| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| page_id     | int     |

(user_id, page_id) is the primary key for this table.

Each row of this table indicates that user_id likes page_id.
 
Write an SQL query to recommend pages to the user with user_id = 1 using the pages that your friends liked. It should not recommend pages you already liked.

``` sql 
with t as (user, friends) (
SELECT user1_id as user, DISTINCT user2_id as friends  
FROM Friendship
WHERE user1_id = 1
UNION 
SELECT user2_id as user, DISTINCT user1_id as friends
WHERE user2_id = 1 -- union how columns are named?
)
SELECT pages as recommended_page
FROM LIKES 
WHERE t in (SELECT friends FROM t) AND page_id NOT in (SELECT page_id FROM likes WHERE user_id = 1); 
```

# Question 1270: All People Report to the Given Manager 

-- Table: Employees

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | employee_id   | int     |
-- | employee_name | varchar |
-- | manager_id    | int     |
-- +---------------+---------+

-- employee_id is the primary key for this table.
-- Each row of this table indicates that the employee with ID employee_id and name employee_name reports his
-- work to his/her direct manager with manager_id
-- The head of the company is the employee with employee_id = 1.
 

-- Write an SQL query to find employee_id of all employees that directly or indirectly report their work to the head of the company.

-- Employees table:
-- +-------------+---------------+------------+
-- | employee_id | employee_name | manager_id |
-- +-------------+---------------+------------+
-- | 1           | Boss          | 1          |
-- | 3           | Alice         | 3          |
-- | 2           | Bob           | 1          |
-- | 4           | Daniel        | 2          |
-- | 7           | Luis          | 4          |
-- | 8           | Jhon          | 3          |
-- | 9           | Angela        | 8          |
-- | 77          | Robert        | 1          |
-- +-------------+---------------+------------+

``` sql 
WITH RECURSIVE t (employee_id, manager_id) as 
(SELECT employee_id, manager_id 
FROM employees 
WHERE employee_name = 'boss'

UNION ALL 

SELECT e.employee_id, manager_id 
FROM t, employees as e
WHERE e.manager_id = t.employee_id
)
SELECT employee_id
FROM t; 
```

# Question 1285: Find the Start and End Number of Continuous Ranges

Table: Logs

| Column Name   | Type    |
|---------------+---------|
| log_id        | int     |

id is the primary key for this table.
Each row of this table contains the ID in a log Table.

Since some IDs have been removed from Logs. Write an SQL query to find the start and end number of continuous ranges in table Logs.

Order the result table by start_id.

The query result format is in the following example:

Logs table:

| log_id     |
|------------|
| 1          |
| 2          |
| 3          |
| 7          |
| 8          |
| 10         |

Result table:

| start_id   | end_id       |
|------------|--------------|
| 1          | 3            |
| 7          | 8            |
| 10         | 10           |

The result table should contain all ranges in table Logs.
From 1 to 3 is contained in the table.
From 4 to 6 is missing in the table
From 7 to 8 is contained in the table.
Number 9 is missing in the table.
Number 10 is contained in the table.
  
``` sql 

```

# Question 1308: Running total for different genders 

Table: Scores

| Column Name   | Type    |
|---------------|---------|
| player_name   | varchar |
| gender        | varchar |
| day           | date    |
| score_points  | int     |

(gender, day) is the primary key for this table.
A competition is held between females team and males team.
Each row of this table indicates that a player_name and with gender has scored score_point in someday.
Gender is 'F' if the player is in females team and 'M' if the player is in males team.

Write an SQL query to find the total score for each gender at each day.

``` sql 
SELECT gender, day
       SUM(score_points) OVER (PARTITION BY gender ORDER BY day) as total
FROM scores
ORDER BY 1, 2;  
```

# Question 1321: restaurant growth 

Table: Customer

| Column Name   | Type    |
|---------------|---------|
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |

(customer_id, visited_on) is the primary key for this table.
This table contains data about customer transactions in a restaurant.
visited_on is the date on which the customer with ID (customer_id) have visited the restaurant.
amount is the total paid by a customer.

You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).

Write an SQL query to compute moving average of how much customer paid in a 7 days window (current day + 6 days before) .

``` sql 
SELECT visited_on, 
       SUM(amount) OVER (PARTITION BY visited_on) as amount, 
       AVG(SUM(amount) OVER (PARTITION BY visited_on ROWS 6 PRECEDING) as average_amount
FROM customer; 
```

# Question 1341: movie rating

-- Table: Movies

-- | Column Name   | Type    |
-- +---------------+---------+
-- | movie_id      | int     |
-- | title         | varchar |
-- +---------------+---------+
-- movie_id is the primary key for this table.
-- title is the name of the movie.
-- Table: Users

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | user_id       | int     |
-- | name          | varchar |
-- +---------------+---------+
-- user_id is the primary key for this table.
-- Table: Movie_Rating

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | movie_id      | int     |
-- | user_id       | int     |
-- | rating        | int     |
-- | created_at    | date    |
-- +---------------+---------+
-- (movie_id, user_id) is the primary key for this table.
-- This table contains the rating of a movie by a user in their review.
-- created_at is the user's review date. 
 

-- Write the following SQL query:

-- Find the name of the user who has rated the greatest number of the movies.
-- In case of a tie, return lexicographically smaller user name.

-- Find the movie name with the highest average rating in February 2020.
-- In case of a tie, return lexicographically smaller movie name.


``` sql
SELECT name 
FROM  
    (SELECT u.name as name, COUNT(movie_id) as movie_rated   
     FROM users as u JOIN movie_rating as mr
             ON u.user_id = mr.user_id
     GROUP BY u.name
     ORDER BY 2, 1) as t1
LIMIT 1
UNION 
SELECT title
FROM 
    (SELECT m.title as title, AVG(rating) as avg_ratings
    FROM movies m as JOIN movie_rating as mr
         ON m.movie_id = mr.movie_id 
    WHERE YEAR(mr.created_at) = 2020
    GROUP BY m.title
    ORDER BY 2 DESC, 1)
LIMIT 1; 
```

# Question 1355: activity participants

Table: Friends

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |
| activity      | varchar |

id is the id of the friend and primary key for this table.
name is the name of the friend.
activity is the name of the activity which the friend takes part in.

Table: Activities

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| name          | varchar |

id is the primary key for this table.
name is the name of the activity.

Write an SQL query to find the names of all the activities with neither maximum, nor minimum number of participants.

``` sql 
WITH t as (
SELECT activity, COUNT(name) as counts   
FROM friends 
GROUP BY activity), 
SELECT activity
FROM t 
WHERE counts NOT in 
            (SELECT max(counts), min(counts) 
             FROM friends); 
```

# Question 1364: Number of Trusted Contacts of a Customer 

``` sql 

```

# Question 1393: Capital Gain/Loss

Table: Stocks

| Column Name   | Type    |
|---------------|---------|
| stock_name    | varchar |
| operation     | enum    |
| operation_day | int     |
| price         | int     |

(stock_name, day) is the primary key for this table.
The operation column is an ENUM of type ('Sell', 'Buy')
Each row of this table indicates that the stock which has stock_name had an operation on the day operation_day with the price.

It is guaranteed that each 'Sell' operation for a stock has a corresponding 'Buy' operation in a previous day.

Write an SQL query to report the Capital gain/loss for each stock.

The capital gain/loss of a stock is total gain or loss after buying and selling the stock one or many times.

Return the result table in any order.

``` sql 
WITH t as 
(
SELECT stock_name, operation, operation_day, price 
       LAG(price) OVER (PARTITION BY stock_name ORDER BY operation_day) as sell_price -- assume buy and sell in different days
FROM stocks)
SELECT stock_name, SUM(sell_price - price) as  capital_gain_loss
FROM t
WHERE operation = 'buy'
GROUP BY stock_name;


-- better? 
SELECT stock_name,
        SUM(CASE WHEN operation = 'Buy' THEN - price ELSE price END) as capital_gain_loss  
FROM stocks 
GROUP BY stock_name; 
```

# Question 1398: Customers Who Bought Products A and B but Not C

Table: Customers

| Column Name         | Type    |
|---------------------+---------|
| customer_id         | int     |
| customer_name       | varchar |

customer_id is the primary key for this table.
customer_name is the name of the customer.
 
Table: Orders

| Column Name   | Type    |
|---------------|---------|
| order_id      | int     |
| customer_id   | int     |
| product_name  | varchar |

order_id is the primary key for this table.
customer_id is the id of the customer who bought the product "product_name".
 
Write an SQL query to report the customer_id and customer_name of customers who bought products "A", "B" but did not buy the product "C" since we want to recommend them buy this product.

Return the result table ordered by customer_id.

The query result format is in the following example.

Customers table:

| customer_id | customer_name |
|-------------|---------------|
| 1           | Daniel        |
| 2           | Diana         |
| 3           | Elizabeth     |
| 4           | Jhon          |


Orders table:

| order_id   | customer_id  | product_name  |
|------------|--------------|---------------|
| 10         |     1        |     A         |
| 20         |     1        |     B         |
| 40         |     1        |     C         |
| 50         |     2        |     A         |
| 60         |     3        |     A         |
| 70         |     3        |     B         |
| 80         |     3        |     D         |
| 90         |     4        |     C         |

Result table:
|-------------|---------------|
| customer_id | customer_name |
|-------------|---------------|
| 3           | Elizabeth     |

Only the customer_id with id 3 bought the product A and B but not the product C.

``` sql
-- minus, INTERESECT
WITH t (customert_id) as (
SELECT 
SELECT customer_id 
FROM orders 
WHERE Product_name = 'A'
INTERSECT 
SELECT customer_id 
FROM orders 
WHERE Product_name = 'B'
MINUS
SELECT customer_id 
FROM orders 
WHERE Product_name = 'c') 
SELECT t.id, customers.customer_name, 
FROM t, customers
WHERE t.customer_id = customers.customer_id; 
```

# Question 1421: NPV Queries 

Table: NPV

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| year          | int     |
| npv           | int     |

(id, year) is the primary key of this table.
The table has information about the id and the year of each inventory and the corresponding net present value.
 
Table: Queries

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| year          | int     |

(id, year) is the primary key of this table.
The table has information about the id and the year of each inventory query.
 
Write an SQL query to find the npv of all each query of queries table.

``` sql 
SELECT queries.id, queries.year, COALESCE(npv.npv, 0) as npv 
FROM queries LEFT JOIN NPV 
     ON queries.id =  npv.id AND queries.year = npv.year; 
```

# Question 1440: Evaluate Boolean Expression

Table Variables:

| Column Name   | Type    |
|---------------|---------|
| name          | varchar |
| value         | int     |

name is the primary key for this table.

This table contains the stored variables and their values.

Table Expressions:

| Column Name   | Type    |
|---------------|---------|
| left_operand  | varchar |
| operator      | enum    |
| right_operand | varchar |

(left_operand, operator, right_operand) is the primary key for this table.
This table contains a boolean expression that should be evaluated.
operator is an enum that takes one of the values ('<', '>', '=')
The values of left_operand and right_operand are guaranteed to be in the Variables table.
 
Write an SQL query to evaluate the boolean expressions in Expressions table.


``` sql 

```

# Question 1445: Apples & Oranges

Table: Sales

| Column Name   | Type    |
|---------------+---------+
| sale_date     | date    |
| fruit         | enum    | 
| sold_num      | int     | 

-- (sale_date,fruit) is the primary key for this table.
-- This table contains the sales of "apples" and "oranges" sold each day.
 
-- Write an SQL query to report the difference between number of apples and oranges sold each day.

-- Return the result table ordered by sale_date in format ('YYYY-MM-DD').

``` sql 

```

# Question 1454: Active Users 

Table Accounts:

| Column Name   | Type    |
| id            | int     |
| name          | varchar |

-- the id is the primary key for this table.
-- This table contains the account id and the user name of each account.
 

-- Table Logins:

| Column Name   | Type    |
|---------------+---------+
| id            | int     |
| login_date    | date    |

-- There is no primary key for this table, it may contain duplicates.
-- This table contains the account id of the user who logged in and the login date. A user may log in multiple times in the day.
 

-- Write an SQL query to find the id and the name of active users.

-- Active users are those who logged in to their accounts for 5 or more consecutive days.

-- Return the result table ordered by the id. 

``` sql 

```

# Question 1459: Rectangles Area

-- Table: Points

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | id            | int     |
-- | x_value       | int     |
-- | y_value       | int     |
-- +---------------+---------+
-- id is the primary key for this table.
-- Each point is represented as a 2D Dimensional (x_value, y_value).
-- Write an SQL query to report of all possible rectangles which can be formed by any two points of the table. 

-- Each row in the result contains three columns (p1, p2, area) where:

-- p1 and p2 are the id of two opposite corners of a rectangle and p1 < p2.
-- Area of this rectangle is represented by the column area.
-- Report the query in descending order by area in case of tie in ascending order by p1 and p2.

-- Points table:
-- +----------+-------------+-------------+
-- | id       | x_value     | y_value     |
-- +----------+-------------+-------------+
-- | 1        | 2           | 8           |
-- | 2        | 4           | 7           |
-- | 3        | 2           | 10          |
-- +----------+-------------+-------------+

-- Result table:
-- +----------+-------------+-------------+
-- | p1       | p2          | area        |
-- +----------+-------------+-------------+
-- | 2        | 3           | 6           |
-- | 1        | 2           | 2           |
-- +----------+-------------+-------------+

-- p1 should be less than p2 and area greater than 0.
-- p1 = 1 and p2 = 2, has an area equal to |2-4| * |8-7| = 2.
-- p1 = 2 and p2 = 3, has an area equal to |4-2| * |7-10| = 6.
-- p1 = 1 and p2 = 3 It's not possible because the rectangle has an area equal to 0.

``` sql 

```

# Question 1468: Calculate Salaries

-- Table Salaries:

-- +---------------+---------+
-- | Column Name   | Type    |
-- +---------------+---------+
-- | company_id    | int     |
-- | employee_id   | int     |
-- | employee_name | varchar |
-- | salary        | int     |
-- +---------------+---------+
-- (company_id, employee_id) is the primary key for this table.
-- This table contains the company id, the id, the name and the salary for an employee.
 

-- Write an SQL query to find the salaries of the employees after applying taxes.

-- The tax rate is calculated for each company based on the following criteria:

-- 0% If the max salary of any employee in the company is less than 1000$.
-- 24% If the max salary of any employee in the company is in the range [1000, 10000] inclusive.
-- 49% If the max salary of any employee in the company is greater than 10000$.
-- Return the result table in any order. Round the salary to the nearest integer.

-- The query result format is in the following example:

-- Salaries table:
-- +------------+-------------+---------------+--------+
-- | company_id | employee_id | employee_name | salary |
-- +------------+-------------+---------------+--------+
-- | 1          | 1           | Tony          | 2000   |
-- | 1          | 2           | Pronub        | 21300  |
-- | 1          | 3           | Tyrrox        | 10800  |
-- | 2          | 1           | Pam           | 300    |
-- | 2          | 7           | Bassem        | 450    |
-- | 2          | 9           | Hermione      | 700    |
-- | 3          | 7           | Bocaben       | 100    |
-- | 3          | 2           | Ognjen        | 2200   |
-- | 3          | 13          | Nyancat       | 3300   |
-- | 3          | 15          | Morninngcat   | 1866   |
-- +------------+-------------+---------------+--------+

-- Result table:
-- +------------+-------------+---------------+--------+
-- | company_id | employee_id | employee_name | salary |
-- +------------+-------------+---------------+--------+
-- | 1          | 1           | Tony          | 1020   |
-- | 1          | 2           | Pronub        | 10863  |
-- | 1          | 3           | Tyrrox        | 5508   |
-- | 2          | 1           | Pam           | 300    |
-- | 2          | 7           | Bassem        | 450    |
-- | 2          | 9           | Hermione      | 700    |
-- | 3          | 7           | Bocaben       | 76     |
-- | 3          | 2           | Ognjen        | 1672   |
-- | 3          | 13          | Nyancat       | 2508   |
-- | 3          | 15          | Morninngcat   | 5911   |
-- +------------+-------------+---------------+--------+
-- For company 1, Max salary is 21300. Employees in company 1 have taxes = 49%
-- For company 2, Max salary is 700. Employees in company 2 have taxes = 0%
-- For company 3, Max salary is 7777. Employees in company 3 have taxes = 24%
-- The salary after taxes = salary - (taxes percentage / 100) * salary
-- For example, Salary for Morninngcat (3, 15) after taxes = 7777 - 7777 * (24 / 100) = 7777 - 1866.48 = 5910.52, which is rounded to 5911.

``` sql 
-- CTE, CASE WHEN
WITH t as 
(
SELECT company_id, employee_id, employee_name, salary, 
       MAX(salary) OVER (PARTITION BY company_id) as max_salary
FROM salaries 
)
SELECT company_id, employee_id, employee_name, 
       CASE 
            WHEN max_salary < 1000 THEN salary
            WHEN max_salary >= 1000 AND max_salary <= 10000 THEN salary*0.24
            ELSE salary*0.49
       END as salaries
FROM t; 
```


# Question 1501: Countries You Can Safely Invest In 

-- Table Person:

-- +----------------+---------+
-- | Column Name    | Type    |
-- +----------------+---------+
-- | id             | int     |
-- | name           | varchar |
-- | phone_number   | varchar |
-- +----------------+---------+
-- id is the primary key for this table.
-- Each row of this table contains the name of a person and their phone number.
-- Phone number will be in the form 'xxx-yyyyyyy' where xxx is the country code (3 characters) and yyyyyyy is the 
-- phone number (7 characters) where x and y are digits. Both can contain leading zeros.
-- Table Country:

-- +----------------+---------+
-- | Column Name    | Type    |
-- +----------------+---------+
-- | name           | varchar |
-- | country_code   | varchar |
-- +----------------+---------+
-- country_code is the primary key for this table.
-- Each row of this table contains the country name and its code. country_code will be in the form 'xxx' where x is digits.
 

-- Table Calls:

-- +-------------+------+
-- | Column Name | Type |
-- +-------------+------+
-- | caller_id   | int  |
-- | callee_id   | int  |
-- | duration    | int  |
-- +-------------+------+
-- There is no primary key for this table, it may contain duplicates.
-- Each row of this table contains the caller id, callee id and the duration of the call in minutes. caller_id != callee_id
-- A telecommunications company wants to invest in new countries. The country intends to invest in the countries where the average call duration of the calls in this country is strictly greater than the global average call duration.

-- Write an SQL query to find the countries where this company can invest.

-- Return the result table in any order.

-- The query result format is in the following example.

-- Person table:
-- +----+----------+--------------+
-- | id | name     | phone_number |
-- +----+----------+--------------+
-- | 3  | Jonathan | 051-1234567  |
-- | 12 | Elvis    | 051-7654321  |
-- | 1  | Moncef   | 212-1234567  |
-- | 2  | Maroua   | 212-6523651  |
-- | 7  | Meir     | 972-1234567  |
-- | 9  | Rachel   | 972-0011100  |
-- +----+----------+--------------+

-- Country table:
-- +----------+--------------+
-- | name     | country_code |
-- +----------+--------------+
-- | Peru     | 051          |
-- | Israel   | 972          |
-- | Morocco  | 212          |
-- | Germany  | 049          |
-- | Ethiopia | 251          |
-- +----------+--------------+

-- Calls table:
-- +-----------+-----------+----------+
-- | caller_id | callee_id | duration |
-- +-----------+-----------+----------+
-- | 1         | 9         | 33       |
-- | 2         | 9         | 4        |
-- | 1         | 2         | 59       |
-- | 3         | 12        | 102      |
-- | 3         | 12        | 330      |
-- | 12        | 3         | 5        |
-- | 7         | 9         | 13       |
-- | 7         | 1         | 3        |
-- | 9         | 7         | 1        |
-- | 1         | 7         | 7        |
-- +-----------+-----------+----------+

-- Result table:
-- +----------+
-- | country  |
-- +----------+
-- | Peru     |
-- +----------+
-- The average call duration for Peru is (102 + 102 + 330 + 330 + 5 + 5) / 6 = 145.666667
-- The average call duration for Israel is (33 + 4 + 13 + 13 + 3 + 1 + 1 + 7) / 8 = 9.37500
-- The average call duration for Morocco is (33 + 4 + 59 + 59 + 3 + 7) / 6 = 27.5000 
-- Global call duration average = (2 * (33 + 3 + 59 + 102 + 330 + 5 + 13 + 3 + 1 + 7)) / 20 = 55.70000
-- Since Peru is the only country where average call duration is greater than the global average, it's the only recommended country.

``` sql 

```

# Question 1532: The most recent three orders



``` sql 
-- most recent 3 order, ranking, easy
WITH t as 
(
SELECT c.customer_id as customer_id, c.name as name, o.order_id as order_id, o.order_date as order_date
       RANK() OVER (PARTITION c.customer_id ORDER BY o.order_date DESC) as rank_orders
FROM customers as c LEFT JOIN orders as o 
     ON c.customer_id = o.customer_id
)
SELET customer_id, name, order_id, order_date
FROM t
WHERE rank_orders <= 3;  
-- how rank() works if the order by column is null  
``` 

# Question 1549: The most recent orders for each product 

``` sql 
WITH t as 
(
SELECT p.product_id as product_id, p.product_name as product_name, o.order_id as order_id
       o.order_date as order_date,
       RANK() OVER (PARTITION BY p.product_id ORDER BY o.order_date DESC) as rank_orders
FROM products as p LEFT JOIN orders as o 
     ON p.product_id = o.product_id 
) 
SELECT product_id, product_name, order_id, order_date  
FROM t
WHERE rank_orders = 1; 
```

# Question 1555: Bank Account Summary

``` sql 
-- key is to find the amount left after all trans
WITH t1 as (
SELECT paid_to as receiver, SUM(amount) as amount_received
FROM transactions 
GROUPY BY paid_to),
t2 as (
SELECT paid_by as sender, SUM(amount) as amount_sent
FROM transactions
GROUPY BY paid_by), 
t3 as (
SELECT t1.receiver as user_id, (amount_received - amount_sent) as amount_after_trans
FROM t1, t2 
WHERE t1.receiver = t2.sender) 
SELECT u.user_id as id, u.user_name as name, 
       IFNULL(t3.amount_after_trans, u.credit) as credit,   
       CASE 
            WHEN IFNULL(t3.amount_after_trans, u.credit) >= u.credit THEN 'NO'
            ELSE 'YES'
        END as credit_limit_breached
FROM users as u LEFT JOIN t3 
     ON u.user_id = t3.user_id 
```

# Question 1596: Most Frequent Products of Each Customer

``` sql 
WITH t
as (
SELECT customer_id, product_id, COUNT(order_id) as num_of_orders
       rank() OVER (PARTITION BY customer_id ORDER BY COUNT(order_id) DESC) as ranks 
FROM orders
GROUP BY customer_id, product_id) 
SELECT users.customer_id as customer_id,
       t.product_id as product_id
),
SELECT users.customer_id, t.product_id as product_id, t.product_name as product_name     
FROM users LEFT JOIN t 
     ON users.customer_id = t.customer_id 
     JOIN products 
     ON t.product_id = products.product_id
WHERE r.ranks = 1;     
```


# Question 1613: Find the Missing IDs

``` sql 

```