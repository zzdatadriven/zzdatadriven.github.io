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
     AND today.date > yesterday.date 
WHERE today.temperature > yesterday.temperature; 
```
