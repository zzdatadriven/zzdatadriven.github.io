---
name: LeetCode SQL Questions 
tools: [SQL]
image: https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80
description: I tried to answer all the SQL query questions on LeetCode in order to refresh my skills. 
---
# **LeetCode**
Easy: 
[[175] :heavy_check_mark:](#question-175-combine-two-tables)
[[176] :heavy_check_mark:](#question-176-second-highest-salary)
[[177] :heavy_check_mark:](#question-177-nth-highest-salary)
[[178] :question:](#question-178-rank-scores)
[[180] :heavy_check_mark:](#question-180-consecutive-numbers)
[[181] :heavy_check_mark:](#question-181-employees-earning-more-than-their-managers)
[[182] :heavy_check_mark:](#question-182-duplicate-emails)
[[183] :heavy_check_mark:](#question-183-customers-who-never-order)
[[196] :heavy_check_mark:](#question-196-delete-duplicate-emails)
[[197] :heavy_check_mark:](#question-197-rising-temperature)
[[511] :heavy_check_mark:](#question-511-game-play-analysis-1)
[[512] :heavy_check_mark:](#question-512-game-play-analysis-2)
[[577] :heavy_check_mark:](#question-577-employee-bonus) 
[[584] :heavy_check_mark:](#question-584-find-customer-referee)
[[586] :heavy_check_mark:](#question-586-customer-placing-the-largest-number-of-orders)
[[595] :heavy_check_mark:](#question-595-big-countries)
[[596] :heavy_check_mark:](#question-596-classes-more-than-5-students) 
[[597] :heavy_check_mark:](#question-597-friend-requests-1)
[[603] :heavy_check_mark:](#question-603-consecutive-available-seats)
[[607] :heavy_check_mark:](#question-607-sales-person) 
[[610] :heavy_check_mark:](#question-610-triangle-judgement) 
[[613] :heavy_check_mark:](#question-613-shortest-distance-in-a-line)
[[619] :heavy_check_mark:](#question-619-biggest-single-number)
[[620] :heavy_check_mark:](#question-620-not-boring-movies)
[[627] :heavy_check_mark:](#question-627-swap-salary)
[[1050] :heavy_check_mark:](#question-1050-actors-who-cooperated-with-directors-at-least-three-times)
[[1068] :heavy_check_mark:](#question-1068-product-sales-analysis-1)
[[1069] :heavy_check_mark:](#question-1069-product-sales-analysis-2)
[[1075] :heavy_check_mark:](#question-1075-project-employees-1)
[[1076] :heavy_check_mark:](#question-1076-project-employees-2)
[[1082] :heavy_check_mark:](#question-1082-sales-analysis-1)
[[1083] :heavy_check_mark:](#question-1083-sales-analysis-2)
[[1084] :heavy_check_mark:](#question-1084-sales-analysis-3)
[[1113] :question:](#question-1113-number-of-comments-per-post)
[[1141] :heavy_check_mark:](#question-1141-user-activity-for-past-30-days-1)
[[1142] :heavy_check_mark:](#question-1142-user-activity-for-past-30-days-2)
[[1148] :heavy_check_mark:](#question-1148-article-views-1)
[[1173] :heavy_check_mark:](#question-1173-immediate-food-delivery)
[[1179] :confused:](#question-1179-reformat-department-table)
[[1121] :confused:](#question-1121-queries-quality-and-percentage)
[[1241] :question:](#question-1241-number-of-comments-per-post)
[[1251]](#question-1251-average-selling-price)
[[1280]](#question-1280-students-and-examinations)
[[1294]](#question-weather-type-in-each-country)

Medium: 

------------------------------------------------------------------------------------------------

# Question 1294: Weather type in each country 


# Question : Highest grade for each student 


# Question : Get the second most recent activity 

``` sql 

```

# Question 603: Consecutive available seats

``` sql 
SELECT DISTINCT c1.seat_id 
FROM cinema as c1 LEFT JOIN cinema as c2 
     ON c1.seat_id = c2.seat_id - 1
WHERE c1.free = 1 AND c2.free = 1
ORDER BY 1; 
```

# Question : Get highest answer rate question

``` sql 

```

# Question : Game Play Analysis 4

``` sql 
WITH t as (SELECT t1.player_id as id, COUNT(t2.event_date) as count_consecutive_days
FROM activity as t1 LEFT JOIN activity as t2 
     ON date_add(t1.event_date, INTERVAL 1 DAY) = t2.event_date
GROUP BY t1.player_id)
SELECT ROUND(COUNT(count_consecutive_days)/COUNT(*), 2) as fraction
FROM t 
WHERE count_consecutive_days > 0
```

# Question : Game Play Analysis 3

``` sql
-- cumulative sum 
SELECT player_id,
       event_date,
       sum(games_played) over (partition by player_id order by event_date) as games_played_so_far
FROM activity
ORDER BY 1, 2;  
```

# Question : Friend Requests 2 

``` sql
SELECT id, MAX(COUNT(DISTINCT friend)) as num_friends
FROM 
    (SELECT request_id as id, accepter_id as friend  
    FROM request_accepted
    UNION ALL 
    SELECT accepter_id as id, request_id as friend 
    FROM request_accepted) as t 
GROUP BY id 
```

# Question : Find the start and end number of continuous ranges 

``` sql 

```

# Question : Exchange Seats 

``` sql 

```

# Question : Evaluate Boolean Expressions 

``` sql 
```

# Question : Department Highest Salary 

# Question : Customers who bought all products 

# Question : Customers who bought a, b but not c 

``` sql 

```

# Question : Countries you can safely invest in 

``` sql 
  
```

# Question : Count student number in departments 

``` sql 
SELECT d.dept_name, COUNT(s.student_id)
FROM department as d LEFT JOIN student as s 
     ON d.dept_id = s.dept_id 
GROUP BY d.dept_id
ORDER BY COUNT(s.student_id) DESC, d.dept_name; 
```
# Question : Consecutive Numbers

``` sql 
SELECT DISTINCT l1.id 
FROM logs l1 JOIN logs l2 
     ON l1.num = l2.num AND l2.id = l3.id - 1 
     JOIN logs l3 
     ON l2.num = l3.num AND l2.id = l3.id - 1; 
```

# Question : Capital Gain 


# Question : Calculate Salaries

``` sql 
WITH tax_rates as (
    SELECT company_id, 
           CASE WHEN max_sal < 1000 THEN 0 
                ELIF max_sal >= 1000 AND max_sal <= 10000 THEN 0.24 
                ELSE 0.49
           END as rates     
    FROM (SELECT company_id, max(salary) as max_sal FROM salaries GROUP BY company_id) as t) 
SELECT s.company_id, s.employee_id, s.employee_name, (s.salary*(1-t.rates)) as salary 
FROM salaries as s JOIN tax_rates as t 
     ON s.company_id = t.company_id 
```
# Question 1148: Article Views 1 

# Question : Article Views 2 

``` sql 
SELECT DISTINCT viewer_id as id 
FROM 
    (SELECT view_date, viewer_id 
    FROM views 
    GROUP BY view_date, viewer_id 
    HAVING COUNT(DISTINCT article_id) > 1) at t; 
```

# Question : Apples & Oranges 

``` sql 
SELECT COALESCE(a.sale_date, o.sale_date) as sale_date, 
       (IFNULL(a.sold_num, 0) - IFNULL(o.sold_num, 0)) as diff
FROM (SELCT * from sales where fruit = 'apples') as a 
     OUTTER JOIN 
     (SELECT * FROM sales WHERE fruit = 'orange') as o 
     ON a.sale_date = o.sale_date; 
ORDER BY 1; 
``` 

# Question : All people report to the given manager 

# Question : Activity Participants 

``` sql 
with t as (SELECT a.name as activity, COUNT(*) as counts 
FROM activities as a LEFT JOIN friends as f 
     ON a.name = f.activity 
GROUP BY a.name) 
SELECT activity
FROM t 
WHERE t.counts NOT IN (SELECT max(counts), min(counts) FROM t); 
``` 

# Question : Active Businesses 

``` sql 
SELECT DISTINCT business_id
FROM events as e INNER JOin (SELECT event_type, avg(occurances) as avg_occ 
                        FROM event_type 
                        GROUP BY event_type) as t 
     ON e.event_type = t.event_type 
WHERE e.occurances > t.avg_occ; 
```

# Question : Active Users 

``` sql 
WITH temp (SELECT DISTINCT t1.id  
FROM logins t1 JOIN logins t2 
     ON t2.login_date = DATE_ADD(t1.login_date, INTERVAL +1 DAY) AND t2.id = t1.id  
     JOIN logins t3 
     ON t3.login_date = DATE_ADD(t2.login_date, INTERVAL +1 DAY) AND t3.id = t2.id  
     JOIN logins t4 
     ON t4.login_date = DATE_ADD(t3.login_date, INTERVAL +1 DAY) AND t4.id = t3.id  
     JOIN logins t5 
     ON t5.login_date = DATE_ADD(t4.login_date, INTERVAL +1 DAY) AND t5.id = t4.id) 
SELECT t.id as id, a.name as name 
FROM accounts as a JOIN t 
     ON a.id = t.id
ORDER BY t.id; 
```
----------------------------------------------------------------------------------------

# Question 1050: Actors who cooperated with Directors at least three times 

``` sql 
SELECT actor_id, director_id 
FROM actirdirector 
GROUP BY actor_id, director_id
HAVING COUNT(*) >= 3; 
``` 

# Question 1251: Average selling price

# Question 595: Big Countries 

``` sql
SELECT name, population, area
FROM world 
WHERE area > 3,000,000 or population > 25,000,000; 
``` 

# Question 619: Biggest Single number 

``` sql
SELECT MAX(num)
FROM 
    (SELECT num 
    FROM numbers
    GROUP BY num
    HAVING COUNT(num) = 1) as t;  
```

# Question 596: Classes more than 5 students 

``` sql 
SELECT class
FROM classes 
GROUP BY class 
HAVING COUNT(DISTINCT student) >= 5;  
``` 

# Question : Create Session bar chart 

``` sql 
WITH t as (SELECT 
            CASE WHEN duration/60 >= 0 and duration/60 < 5 THEN '[0-5>'
                 ELIF duration/60 >= 5 AND duration/60 < 10 THEN '[5, 10>'
                 ELIF duration/60 >= 10 AND duration/60 < 15 THEN '[10, 15>'
                 ELSE '15 minutes or more'  
            END as bin 
           FROM sessions) 
SELECT bin, COUNT(*) as total 
FROM t 
GROUP BY bin; 
``` 

# Question 586: Customer placing the largest number of orders

``` sql 
SELECT customer_name
FROM orders 
GROUP BY customer_number 
ORDER BY COUNT(order_number) 
LIMIT 1; 
```

# Question : Delete duplicate emails 

``` sql 
DELETE FROM person 
WHERE id NOT IN (SELECT MIN(id)
                 FROM person
                 GROUP BY email); 
``` 

# Question 182: Duplicate Emails 

``` sql 
SELECT email 
FROM person 
GROUP BY email 
HAVING COUNT(email) > 1; 
```

# Question 577: Employee Bonus 

``` sql 
SELECT e.name as name, b.bonus as bonus-+
FROM employee as e LEFT JOIN bonus as b 
     ON e.empid = b.empid 
WHERE b.bonus < 1000 or b.bonus IS NULL; 
```

# Question 584: Find Customer Referee 

``` sql 
SELECT name
FROM customer 
WHERE referee_id != 2 or referee IS NULL; 
```

# Question : Find the team size 

``` sql 
SELECT e.employee_id as employee_id, t.team_size as time_size 
FROM employee as e JOIN (SELECT team_id, COUNT(*) as team_size FROM employee GROUP BY                                team_id) as t
     ON e.team_id = t.team_id
```

# Question 597: Friend Requests 1 

``` sql 
SELECT 
FROM 
```

# Question : Friendly Movies streamed list   

``` sql 
SELECT DISTINCT c.title as title 
FROM TVProgram as t JOIN content as c 
     ON t.content_id = c.content_id 
WHERE t.program_date BEWTEEN ('2020-06-01', '2020-06-30')
      AND c.content_type = 'Movies'
      AND c.kids_content = 'Y'; 
```

# Question 511: Game Play Analysis 1 

``` sql 
SELECT player_id, min(event_date)
FROM activity 
GROUP BY player_id 
ORDER BY 1; 
```

# Question 512: Game play analysis 2 

``` sql 
-- return the first login dates for each player 
SELECT ROUND(COUNT(t.event_date)/COUNT(*), 2) as fraction
FROM 
    (SELECT player_id, min(event_date) as event_date   
    FROM activity 
    group by player_id) as t 
    LEFT JOIN activity as a
    ON DATE_ADD(a.event_date, INTERVAL - 1) = t.event_date -- is ok??? 
GROUP BY player_id 
```

# Question : Group sold products by the date 

Write an SQL query to find for each date, the number of distinct products sold and their names. 

``` sql 
SELECT sell_date, COUNT(DISTINCT product), GROUP_CONCAT(DISTINCT product) as products    
FROM activities 
GROUP BY sell_date;  
```

# Question 1173: Immediate food delivery


``` sql 
SELECT ROUND(COUNT(CASE WHEN order_date = customer_pref_delivery_date THEN 1 ELSE O                 END)/COUNT(*), 2) as immediate_percentage
FROM delivery 
GROUP BY delivery_id;  
```

# Question : List the products ordered in a period

``` sql 
SELECT p.product_name, o.unit
FROM orders as o JOIN products as p 
     ON o.product_id = p.product_id 
WHERE o.order_date BETWEEN ('2020-2-1', '2020-2-28') 
      AND o.unit >= 100;  
```

# Question 620: Not Boring movies 

``` sql 
SELECT id, movie, description, rating   
FROM cinema
WHERE id%2 = 1  AND description <> 'boring'; 
```

# Question 1241: Number of comments per post 

``` sql 
SELECT s1.sub_id as post_id, COUNT(DISTINCT s2.parent_id) as num_of_comment  
FROM submission as s1 JOIN submission as s2 
     ON s1.sub_id = s2.parent_id 
GROUP BY s1.sub_id  
```

``` sql 
SELECT a.sub_id, COALESCE(COUNT(DISTINCT s.parent_id), 0) as number_of_comments 
FROM 
    (SELECT * 
    FROM submissions 
    WHERE parent_id IS NULL) as a 
    LEFT JOIN submissions as s
    ON a.sub_id = s.parent_id 
GROUP BY a.sub_id; 
```

# Question 1068: Product Sales Analysis 1 

``` sql 
SELECT p.product_name, s.year, s.price  
FROM sales as s JOIN product as p 
     ON s.product_id = p.product_id 
```

# Question 1069: Product Sales Analysis 2

``` sql 
SELECT p.product_id, sum(s.quantity) as total_quantity 
FROM product as p LEFT JOIN sales as s
     ON p.product_id = s.product_id 
GROUP BY product_id
```

# Question 1075: Project Employees 1 


# Question 1076: Project Employees 2 

``` sql
SELECT project_id
FROM ( 
    SELECT project_id, count(employee_id) as num_emp   
    FROM project 
    GROUP BY project_id
    ORDER BY count(employee_id) DESC) as t
WHERE 
``` 


# Question 1121: Queries quality and percentage

# Question 1179: Reformat department table 

``` sql
-- PIVOT???? 
SELECT 
FROM
```

# Question : Replace employee id with unique identifier 

``` sql
SELECT em.unique_id, e.name 
FROM employees as e LEFT JOIN employeeuni as em 
     ON e.id = em.id
```

# Question : Reported posts 

``` sql 
SELECT extra as report_reason, count(distinct post_id) as count_post 
FROM actions 
WHERE action_date = '2019-07-04' AND action = 'report' 
GROUP BY extra
```

# Question 1082: Sales Analysis 1

``` sql
SELECT s.seller_id, SUM(p.unit*s.quantity) as total_sales 
FROM sales as s JOIN product as p 
     ON s.product_id = p.product_id 
GROUP BY s.seller_id
ORDER BY SUM(p.unit*s.quantity) DESC
LIMIT 1; 

--i just relize, no need join here, sum price and reorder it. 
```

# Question 1083: Sales Analysis 2   

``` sql
--????? 
WITH t as (
    SELECT * 
    FROM sale as s inner join product as p 
         ON s.product_id = p.product_id
)

SELECT 
FROM t as t1 JOIN t as t2 
     ON t1.product_id = t2.production_id 
     AND t.production
     
```

# Question 1084: Sales Analysis 3 

``` sql
SELET DISTINCT p.product_id, product_name 
FROM product as p INNER JOIN sales as s 
     ON p.product_id = s.product_id 
WHERE s.sale_date BETWEEN ('2019-01-01', '2019-03-31'); 
``` 

# Question 607: Sales Person 

``` sql 
SELECT name 
FROM salesperson 
WHERE sales_id NOT IN 
    (SELECT DISTINCT sales_id
    FROM orders as o 
         JOIN company as c 
         ON c.com_id = o.com_id 
    WHERE c.name = 'RED'); 
```

# Question 613: Shortest Distance in a line 

``` sql 
SELECT min(abs(abs(p1.x) - abs(p2.x))) as minimum  
FROM points as p1 INNER JOIN points as p2
     ON p1.x <> p2.x; -- since no duplicated values in x. 
```


# Question 1280: Students and Examinations 

``` sql
--??????????? 
SELECT s.student_id, s.student_name, e.subject_name, IFNULL(count(*), 0) as attended_exams
FROM (SELECT * FROM students CROSS JOIN subjects) as s 
     LEFT JOIN examinations as e  
     ON s.student_id = e.student_id AND s.subject_name = e.subject_name
GROUP BY s.student_id, e.subject_name;
```

# Question : Students with invalid departments 

``` sql 
-- left join
SELECT students.id, students.name
FROM students LEFT JOIN departments 
     ON students.department_id = departments.id
WHERE departments.id IS NULL;   

-- subquery 
SELECT id, name 
FROM students 
WHERE department_id NOT IN (SELECT id FROM departments); 
```

# Question 627: Swap Salary 

``` sql 
UPDATE 
SET sex = CASE WHEN 'm' THEN 'f'
          ELSE 'm'
          END; 
```

# Question 610: Triangle Judgement 

| x  | y  | z  |
|----|----|----|
| 13 | 15 | 30 |
| 10 | 20 | 15 |

``` sql 
SELECT x, y, z
       CASE 
           WHEN x+y > z and x+z > y and z+y > x THEN 'YES'
           ELSE 'NO'
       END AS triangle
FROM triangle; 
```

# Question : Top Travelers 

Write an SQL query to report the distance traveled by each user. 

``` sql
SELECT users.name, IFNULL(sum(rides.distance), 0) AS distince_traveled,  
FROM users LEFT JOIN rides
     ON users.id = rides.user_id 
GROUP BY users.id
ORDER BY 2, 1;  
```

# Question 1141: User activity for past 30 days 1

Write an SQL query to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on some day if he/she made at least one activity on that day.

**Activity table:**

| user_id | session_id | activity_date | activity_type |
|---------|------------|---------------|---------------|
| 1       | 1          | 2019-07-20    | open_session  |
| 1       | 1          | 2019-07-20    | scroll_down   |
| 1       | 1          | 2019-07-20    | end_session   |
| 2       | 4          | 2019-07-20    | open_session  |
| 2       | 4          | 2019-07-21    | send_message  |
| 2       | 4          | 2019-07-21    | end_session   |
| 3       | 2          | 2019-07-21    | open_session  |
| 3       | 2          | 2019-07-21    | send_message  |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |

**Results:** 

| day        | active_users |
|------------|--------------|
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |

``` sql
SELECT activity_data, COUNT(distinct user_id) 
FROM activity
WHERE activity BETWEEN (DATE_ADD('2019-07-27',INTERVAL -30 DAY), '2019-07-27')
GROUP BY activity_date 
HAVING COUNT(DISTINCT user_id) > 0; 
```

# Question 1142: User activity for past 30 days 2 

Write an SQL query to find the average number of sessions per user for a period of 30 days ending 2019-07-27 inclusively, rounded to 2 decimal places. The sessions we want to count for a user are those with at least one activity in that time period.

**Activity table:**

| user_id | session_id | activity_date | activity_type |
|---------|------------|---------------|---------------|
| 1       | 1          | 2019-07-20    | open_session  |
| 1       | 1          | 2019-07-20    | scroll_down   |
| 1       | 1          | 2019-07-20    | end_session   |
| 2       | 4          | 2019-07-20    | open_session  |
| 2       | 4          | 2019-07-21    | send_message  |
| 2       | 4          | 2019-07-21    | end_session   |
| 3       | 2          | 2019-07-21    | open_session  |
| 3       | 2          | 2019-07-21    | send_message  |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |

**Results:** 

| average_sessions_per_user |
|---------------------------| 
| 1.33                      |

``` sql
SELECT AVG(counts) as average_session_per_user FROM (
    SELECT COUNT(session_id) as counts, usr_id  
    FROM activity 
    WHERE activity_date between (DATE_ADD('2019-07-27', INTERVAL -30 DAY), '2019-07-27')
    GROUP BY user_id) AS a; 
```

# Question 175: Combine Two Tables

**Person:**

| Column Name | Type    |
|-------------|---------|
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |

**Address:**

| Column Name | Type    |
|-------------|---------|
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |

Write a SQL query for a report that provides **FirstName, LastName, City, State** for each person in the Person table, regardless if there is an address for each of those people. 

``` sql
SELECT p.firstname, p.lastname, a.city, s.state 
FROM person as p LEFT JOIN address as a 
     ON p.personid = a.personid; 
```

# Question 176: Second Highest Salary

Write a SQL query to get the second highest salary from the **Employee** table.

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

``` sql
-- subquery
SELECT MAX(salary)
FROM employee 
WHERE salary < (SELECT MAX salary FROM employee);

-- use offset
SELECT DISTINCT salary 
FROM employee 
ORDER BY salary DESC
Limit 1 OFFSET 1; -- LIMIT n-1, OFFSET 1 (return the n highest salary) 
```

# Question 177: Nth Highest Salary

Write a SQL query to get the nth highest salary from the **Employee** table.

| Id | Salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

``` sql 
SELECT DISTINCT salary 
FROM employee 
ORDER BY salary DESC
LIMIT N-1 OFFSET 1; 
```

# Question 178: Rank Scores 

Write a SQL query to rank scores. If there is a tie between two scores, both should have the same ranking. Note that after a tie, the next ranking number should be the next consecutive integer value. In other words, there should be no "holes" between ranks.

| Id | Score |
|----|-------|
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |

For example, given the aboveScorestable, your query should generate the following report (order by highest score):

| Score | Rank |
|-------|------|
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |

``` sql 
-- self join

-- rank function
WITH t as (
SELECT score, DENSE_RANK() OVER (ORDER BY score DESC) ranks 
FROM score)
SELECT score, ranks -- rank is built-in function, so be careful
FROM score 
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

For example, given the above **Logs** table,1is the only number that appears consecutively for at least three times.

| ConsecutiveNums |
|-----------------|
| 1               |

``` sql
SELECT DISTINCT l1.num 
FROM logs as l1 join logs as l2 
     ON l1.id = l2.id - 1 AND l1.num = l2.num
     JOIN logs l3 
     ON l3.id -1 = l2.id AND l3.num = l2.num;  
``` 

# Question 181: Employees Earning More Than Their Managers

The **Employee** table holds all employees including their managers. Every employee has an Id, and there is also a column for the manager Id.

| Id | Name  | Salary | ManagerId |
|----|-------|--------|-----------|
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |

Given the **Employee** table, write a SQL query that finds out employees who earn more than their managers. For the above table, Joe is the only employee who earns more than his manager. 

``` sql
-- self join
SELECT e1.name 
FROM employee as e1 join employee as e2 
     ON e1.managerid = e2.id
WHERE e1.salary > e2.salary; 

-- Non-equi join 
SELECT e1.name 
FROM employee as e1, 
     employee as e2
WHERE e1.managerid = e2.id 
      AND e1.salary > e2.salary; 
```

# Question 182: Duplicate Emails 

Write a SQL query to find all duplicate emails in a table named **Person**.

| Id | Email   |
|----|---------|
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |

``` sql
-- subquery 
SELECT email
FROM person
GROUP BY email 
HAVING COUNT(*) > 1; 
            
-- Non-equi self join
SELECT DISTINCT e1.email    
FROM person as p1 JOIN person as p2 
     ON p1.email = p2.email 
     AND p1.id <> p2.id; 
```

# Question 183: Customers Who Never Order
Suppose that a website contains two tables, the **Customers** table and the **Orders** table. Write a SQL query to find all customers who never order anything.

**Customers:**

| Id | Name  | 
|----|-------|
| 1  | JOE   |  
| 2  | HENRY |
| 3  | SAM   |  
| 4  | MAX   | 

**Orders:**

| Id | CustomerID| 
|----|-----------|
| 1  | 3         |  
| 2  | 1         |

``` sql
-- subquery 
SELECT Name
FROM customers
WHERE ID NOT IN (SELECT DISTINCT ID FROM orders); 

-- join 
SELECT customer.name
FROM customers LEFT JOIN orders 
     ON customers.id = orders.customerid 
groupy by customer.id 
having count(orders.id) = 0; 
```


# Question 184: Department Highest Salary
The **Employee** table holds all employees. Every employee has an Id, and there is also a column for the department Id.


| Id | Name  | Salary | DepartmentId |
|----|-------|--------|--------------|
| 1  | JOE   | 70000  | 1            |
| 2  | HENRY | 80000  | 2            |
| 3  | SAM   | 60000  | 2            |
| 4  | MAX   | 90000  | 1            |
| 5  | JANET | 69000  | 1            |
| 6  | RANDY | 85000  | 1            |

The **Department** table holds all departments of the company.

| ID | NAME  |
|----|-------|
| 1  | IT    |
| 2  | Sales |

Write a SQL query to find employees who have the highest salary in each of the departments. For the above tables, Max has the highest salary in the IT department and Henry has the highest salary in the Sales department. 

# Question 185: Department Top Three Salaries 
The **Employee** table holds all employees. Every employee has an Id, and there is also a column for the department Id.

| Id | Name  | Salary | DepartmentId |
|----|-------|--------|--------------|
| 1  | JOE   | 70000  | 1            |
| 2  | HENRY | 80000  | 2            |
| 3  | SAM   | 60000  | 2            |
| 4  | MAX   | 90000  | 1            |
| 5  | JANET | 69000  | 1            |
| 6  | RANDY | 85000  | 1            |

The **Department** table holds all departments of the company.

| ID | NAME  |
|----|-------|
| 1  | IT    |
| 2  | Sales |

Write a SQL query to find employees who earn the top three salaries in each of the department.

``` sql
WITH rank_table as (
SELECT *, DENSE_RANK() OVER (PARTITION BY e.DepartmentId ORDER BY e.salary DESC) as rank
FROM employee as e JOIN department as d on e.DepartmentId = d.ID
)
SELECT * 
FROM rank_table 
WHERE rank <= 3; 
```

# Question 196: delete duplicate emails
Write a SQL query to delete all duplicate email entries in a table named **Person**, keeping only unique emails based on its smallest Id.

| ID | email             |
|----|-------------------|
| 1  | john@example.com  |
| 2  | sfahn@example.com |
| 3  | john@example.com  |

``` sql
DELETE p1
FROM person as p1 JOIN person as p2 
     on p1.email = p2.email
WHERE p1.ID > p2.ID
``` 

# Question 197: Rising Temperature 
Given a **Weather** table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.

| Id(INT) | Date(DATE) | Temperature(INT) |
|---------|------------|------------------|
| 1       | 2015-01-01 | 10               |
| 2       | 2015-01-02 | 25               |
| 3       | 2015-01-03 | 20               |
| 4       | 2015-01-04 | 30               |

``` sql
SELECT today.data 
FROM weather as today INNER JOIN weather as yesterday 
     ON DATEDIFF(today.date, yesterday.date) = 1 
WHERE today.temperature > yesterday.temperature; 
```
