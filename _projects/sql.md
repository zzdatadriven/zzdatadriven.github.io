---
name: LeetCode SQL Questions 
tools: [SQL]
image: https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80
description: I tried to answer all the SQL query questions on LeetCode in order to refresh my skills. 
---
# **LeetCode**
[[175] :heavy_check_mark:](#question-175-combine-two-tables)
[[176] :heavy_check_mark:](#question-176-second-highest-salary)
[[177] :heavy_check_mark:](#question-177-nth-highest-salary)
[[178] :question:](#question-178-rank-scores)
[[180] :heavy_check_mark:](#question-180-consecutive-numbers)
[[181] :heavy_check_mark:](#question-181-employees-earning-more-than-their-managers)

# Question : Employee Bonus 

``` sql 
SELECT 
FROM employee as e LEFT JOIN bonus as b 
     ON e.empid = b.empid 
WHERE b.bonus < 1000 or b.bonus IS NULL; 
```

# Question : Find Customer Referee 

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

# Question : Friend Requests 1 

# Question : Friendly Movies streamed list   

``` sql 
SELECT DISTINCT c.title as title 
FROM TVProgram as t JOIN content as c 
     ON t.content_id = c.content_id 
WHERE t.program_date BEWTEEN ('2020-06-01', '2020-06-30')
      AND c.content_type = 'Movies'
      AND c.kids_content = 'Y'; 
```

# Question : Game Play Analysis 1 


# Question : Game play analysis 2 


# Question : Group sold products by the date 

Write an SQL query to find for each date, the number of distinct products sold and their names. 

``` sql 
SELECT sell_date, COUNT(DISTINCT product), GROUP_CONCAT(DISTINCT product) as products    
FROM activities 
GROUP BY sell_date;  
```

# Question : Immediate food delivery 

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

# Question : Not Boring movies 

``` sql 
SELECT id, movie, description, rating   
FROM cinema
WHERE id%2 = 1  AND description <> 'boring'; 
```

# Question : Number of comments per post 

``` sql 
SELECT s1.sub_id as post_id, COUNT(DISTINCT s2.parent_id) as num_of_comment  
FROM submission as s1 JOIN submission as s2 
     ON s1.sub_id = s2.parent_id 
GROUP BY s1.sub_id  
```

--
# Question : Product Sales Analysis 1 

``` sql 
SELECT p.product_name, s.year, s.price  
FROM sales as s JOIN product as p 
     ON s.product_id = p.product_id 
```

# Question : Product Sales Analysis 2

``` sql 
SELECT p.product_id, sum(s.quantity) as total_quantity 
FROM product as p LEFT JOIN sales as s
     ON p.product_id = s.product_id 
GROUP BY product_id
```

# Question : Project Employees 1 


# Question : Project Employees 2 

``` sql
SELECT project_id
FROM ( 
    SELECT project_id, count(employee_id) as num_emp   
    FROM project 
    GROUP BY project_id
    ORDER BY count(employee_id) DESC) as t
WHERE 
``` 


# Question : Queries quality and percentage

# Question : Reformat department table 

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

# Question : Sales Analysis 1

``` sql
SELECT s.seller_id, SUM(p.unit*s.quantity) as total_sales 
FROM sales as s JOIN product as p 
     ON s.product_id = p.product_id 
GROUP BY s.seller_id
ORDER BY SUM(p.unit*s.quantity) DESC
LIMIT 1; 

--i just relize, no need join here, sum price and reorder it. 
```

# Question : Sales Analysis 2   

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

# Question : Sales Analysis 3 

``` sql
SELET DISTINCT p.product_id, product_name 
FROM product as p INNER JOIN sales as s 
     ON p.product_id = s.product_id 
WHERE s.sale_date BETWEEN ('2019-01-01', '2019-03-31'); 
``` 

# Question : Sales Person 


# Question : Shortest Distance 

``` sql 
SELECT min(abs(abs(p1.x) - abs(p2.x))) as minimum  
FROM points as p1 INNER JOIN points as p2
     ON p1.x <> p2.x; -- since no duplicated values in x. 
```


# Question : Students and Examinations 

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

# Question : Swap Salary 

``` sql 
UPDATE 
SET sex = CASE WHEN 'm' THEN 'f'
          ELSE 'm'
          END; 
```

# Question : Triangle Judgment 

| x  | y  | z  |
|----|----|----|
| 13 | 15 | 30 |
| 10 | 20 | 15 |

``` sql 
SELECT x, y, z
       CASE 
           WHEN x+y > z and x+z > y and z+y > x THEN 'YES'
           ELIF x = z AND z = y THEN 'YES'
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

# Question : User activity for past 30 days 2 

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

-- equijoin 
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
            
-- self join
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
