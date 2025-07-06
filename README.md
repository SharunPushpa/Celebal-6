# Celebal-6

175.SELECT 
    FirstName, 
    LastName, 
    City, 
    State
FROM Person
LEFT JOIN Address
ON Person.PersonId = Address.PersonId;

176.SELECT MAX(Salary) AS SecondHighestSalary
FROM Employee
WHERE Salary < (
    SELECT MAX(Salary) FROM Employee
);

177.CREATE FUNCTION getNthHighestSalary(@N INT) RETURNS INT AS
BEGIN
    RETURN (
        SELECT DISTINCT Salary
        FROM Employee
        ORDER BY Salary DESC
        OFFSET @N - 1 ROWS FETCH NEXT 1 ROWS ONLY
    );
END

178.SELECT 
    Score,
    DENSE_RANK() OVER (ORDER BY Score DESC) AS Rank
FROM Scores;

180.SELECT DISTINCT l1.Num
FROM Logs l1, Logs l2, Logs l3
WHERE l1.Id = l2.Id - 1 
  AND l2.Id = l3.Id - 1 
  AND l1.Num = l2.Num 
  AND l2.Num = l3.Num;

181.SELECT e1.Name AS Employee
FROM Employee e1
JOIN Employee e2
ON e1.ManagerId = e2.Id
WHERE e1.Salary > e2.Salary;

182.SELECT Email
FROM Person
GROUP BY Email
HAVING COUNT(*) > 1;

183.SELECT Name
FROM Customers
WHERE Id NOT IN (
    SELECT CustomerId FROM Orders
);

184.SELECT d.Name AS Department, e.Name AS Employee, e.Salary
FROM Employee e
JOIN Department d ON e.DepartmentId = d.Id
WHERE (e.Salary, e.DepartmentId) IN (
    SELECT MAX(Salary), DepartmentId
    FROM Employee
    GROUP BY DepartmentId
);

185.SELECT 
    d.Name AS Department,
    e.Name AS Employee,
    e.Salary
FROM (
    SELECT *,
           DENSE_RANK() OVER (PARTITION BY DepartmentId ORDER BY Salary DESC) AS rank
    FROM Employee
) e
JOIN Department d ON e.DepartmentId = d.Id
WHERE e.rank <= 3;

586.SELECT customer_number
FROM Orders
GROUP BY customer_number
ORDER BY COUNT(*) DESC
LIMIT 1;

595.SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;

596.SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;

601.SELECT id, visit_date, people
FROM (
    SELECT *,
           LEAD(people,1) OVER (ORDER BY visit_date) AS next1,
           LEAD(people,2) OVER (ORDER BY visit_date) AS next2
    FROM Stadium
) t
WHERE people >= 100 AND next1 >= 100 AND next2 >= 100;

602.SELECT id
FROM (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id FROM RequestAccepted
) AS all_ids
GROUP BY id
ORDER BY COUNT(*) DESC
LIMIT 1;
