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
