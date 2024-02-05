# Write well documented SQL statements for the following :

## 1. Create a table for Employees of BeachBrainsNITK.

`CREATE TABLE IF NOT EXISTS Employees (
    EmpID INT PRIMARY KEY,
    FirstName VARCHAR(255),
    LastName VARCHAR(255),
    Position VARCHAR(255),
    ManagerID INT,
    JoinAt DATE,
    Salary INT
);`

## 2. Insert tuples with appropriate values as per the assumed attributes, adhering to the Employee hierarchy shown above.

`INSERT INTO Employees VALUES
    (17, 'Shaurya', 'Sharma', 'CEO', NULL, '2018-05-21', 120000),
    (15, 'Sita', 'Bharti', 'CTO', 17, '2018-07-01', 80000),
    (16, 'Mahesh', 'Sirvi', 'CTO', 17, '2018-06-06', 80000),
    (10, 'Jayraj', 'Tripathi', 'Analyst', 15, '2019-03-12', 60000),
    (13, 'Jaiyash', 'Sharma', 'Analyst', 15, '2019-08-21', 60000),
    (11, 'Ayan', 'Ansari', 'Analyst', 16, '2019-02-03', 60000),
    (12, 'Naman', 'Jain', 'Analyst', 16, '2018-05-21', 60000),
    (14, 'Sarang', 'Singh', 'Analyst', 16, '2018-09-27', 60000),
    (1, 'Rishab', 'Arora', 'SDE', 10, '2020-10-30', 40000),
    (2, 'Aman', 'Singh', 'SDE', 10, '2020-05-26', 50000),
    (3, 'Nakul', 'Dhull', 'SDE', 10, '2018-06-17', 60000),
    (4, 'Rajat', 'Dalal', 'SDE', 10, '2021-07-19', 30000),
    (5, 'Sourav', 'Singha', 'SDE', 11, '2023-09-21', 25000),
    (6, 'Pratham', 'Sharma', 'SDE', 11, '2023-01-09', 20000),
    (7, 'Pushti', 'Gupta', 'SDE', 12, '2022-11-16', 60000),
    (8, 'Adarsh', 'Ram', 'SDE', 12, '2020-04-15', 60000);`

## 3. List the names of all Analysts of the BeachBrainsNITK.

`SELECT FirstName, LastName
FROM Employees
WHERE Position = 'Analyst';`

## 4. List the names of all SDEs in reverse alphabetical order.

`SELECT FirstName, LastName
FROM Employees
WHERE Position = 'SDE'
ORDER BY LastName DESC;`

## 5. List the name of CTO of ‘Adarsh Ram’ (EmpID 8).

`SELECT E.FirstName, E.LastName
FROM Employees E
JOIN Employees A ON E.ManagerID = A.EmpID
WHERE A.EmpID = 8 AND E.Position = 'CTO';`

## 6. List the names of all SDEs whose Salary is at least ₹25,000 less than their Analysts.

`SELECT SDE.FirstName, SDE.LastName
FROM Employees SDE
JOIN Employees Analyst ON SDE.ManagerID = Analyst.EmpID
WHERE SDE.Position = 'SDE' AND Analyst.Position = 'Analyst' AND SDE.Salary <= Analyst.Salary - 25000;`

## 7. List the names Employees of BeachBrainsNITK who don’t have any subordinates.

`SELECT E.FirstName, E.LastName
FROM Employees E
LEFT JOIN Employees Subordinate ON E.EmpID = Subordinate.ManagerID
WHERE Subordinate.EmpID IS NULL;`

## 8. List the names of Analysts of BeachBrainsNITK who don’t have any subordinates.

`SELECT A.FirstName, A.LastName
FROM Employees A
LEFT JOIN Employees Subordinate ON A.EmpID = Subordinate.ManagerID
WHERE A.Position = 'Analyst' AND Subordinate.EmpID IS NULL;`

## 9. List the names of Employees of BeachBrainsNITK who directly or indirectly serve under supervision of ‘Sita Bharti’ (EmpID 15).

`WITH RECURSIVE EmployeeHierarchy AS (
SELECT EmpID, FirstName, LastName, ManagerID
FROM Employees
WHERE EmpID = 15

    UNION

    SELECT E.EmpID, E.FirstName, E.LastName, E.ManagerID
    FROM Employees E
    JOIN EmployeeHierarchy EH ON E.ManagerID = EH.EmpID

)

SELECT FirstName, LastName
FROM EmployeeHierarchy;`

## 10. List the names of all Employees of BeachBrainsNITK who have at least 3 subordinates.

`WITH SubordinateCount AS (
SELECT ManagerID, COUNT(_) AS NumSubordinates
FROM Employees
GROUP BY ManagerID
HAVING COUNT(_) >= 3
)

SELECT E.FirstName, E.LastName
FROM Employees E
JOIN SubordinateCount SC ON E.EmpID = SC.ManagerID;`
