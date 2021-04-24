---
name: LeetCode SQL Questions 
tools: [SQL]
image: https://images.unsplash.com/photo-1489875347897-49f64b51c1f8?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80
description: I tried to answer all the SQL query questions on LeetCode preparing for job interview and refreshing my skills.  
---
# Question 183: Customers Who Never Order
Suppose that a website contains two tables, theCustomerstable and theOrderstable. Write a SQL query to find all customers who never order anything.



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