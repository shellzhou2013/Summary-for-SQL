Leetcode 175. Combine two tables. Get the four columns of all the primary keys of the person table.
```
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
;
```
Leetcode 176. Second highest salary. Find the second highest salary. If there is no second highest, return Null. For this problem, we can use a ifnull clause. As for how to pick the second, we can use descend order, limit one offset 1. Similarly, if you want to get the Mth highest, offset M-1.

Or we can use select max where salary < (select max(salary) from ... )
```
SELECT
IFNULL(
(SELECT DISTINCT Salary
 FROM Employee
 ORDER BY Salary DESC
 LIMIT 1 OFFSET 1),
 NULL) AS SecondHighestSalary
```
```
SELECT
IFNULL(
(SELECT MAX(Salary)
 FROM Employee
 WHERE Salary < (SELECT MAX(Salary) FROM Employee)),
 NULL) AS SecondHighestSalary;
```
Leetcode 178. Rank Scores. We want to rank the scores. If we have same scores, rank them the same. And if we have rank of 1,2,2, the next is still 3.
```
select A.Score, count(B.Score) as Rank
from Scores as A, (select distinct Score
from Scores) as B
where A.score <= B.Score
group by A.Id
order by A.Score desc;
```

Leetcode 180: Consecutive numbers. Find all numbers that appear at least three times consecutively. An easy way is to observe that the ids are consecutive. A more conserved way is to use lag and over
```
SELECT DISTINCT
    l1.Num AS ConsecutiveNums
FROM
    Logs l1,
    Logs l2,
    Logs l3
WHERE
    l1.Id = l2.Id - 1
    AND l2.Id = l3.Id - 1
    AND l1.Num = l2.Num
    AND l2.Num = l3.Num
    ;
```

Leetcode 181. Employees Earning More Than Their Managers. First method is just using a where clause. Another method is to use inner join.
```
select a.Name as 'Employee'
from
employee as a,
employee as b
where
a.managerid = b.id
and a.salary > b.salary;
```
```
SELECT
a.NAME AS Employee
FROM Employee AS a INNER JOIN Employee AS b
ON a.ManagerId = b.Id
AND a.Salary > b.Salary
;
```
Leetcode 182: duplicated emails: find all duplicate emails. Using group by and filter that count > 1. First is to use 'having' clause. The general way is to create a temp table.
```
select email
from person
group by email
having (count(email) > 1);
```
```
select email from
(select email, count(email) as cnt
from person
group by email) as stats
where cnt > 1;
```
Leetcode 183. customers who never ordered: two tables called customers and orders, find all names from customers who don't order.
```
select name as customers
from customers
where id not in
(select customerid from orders);
```
Leetcode 184. First in where clause, find the highest salary using group by. Then inner join two table and select the highest.
```
select department.name as Department, employee.name as Employee, employee.salary as Salary
from
(employee inner join department
 on employee.departmentid = department.id
)
where (employee.departmentid, employee.salary) in
(select departmentid, max(salary)
 from employee
 group by departmentid
)
```

