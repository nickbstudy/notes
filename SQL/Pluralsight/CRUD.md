In SQL:

Create = `INSERT`
Read = `SELECT`
Update = `UPDATE`
Delete = `DELETE`

#### Insert
All fields are not always required, types must match of course.
```
INSERT INTO Employees (FirstName, HireDate, IsFullTime)
VALUES ('John', '2024-06-11', TRUE);
```
Multiple simultaneous values can be added:
```
INSERT INTO Employees (FirstName, HireDate, IsFullTime)
VALUES ('John', '2024-06-11', TRUE),
       ('Joe", '2024-01-01', FALSE);
```

#### Update
Technically a `WHERE` clause is not required, but this should almost always be included - otherwise changes will be applied to the entire table.
```
UPDATE Employee
SET Status = 'Retired', Salary = 0, DepartmentID = NULL
WHERE EmployeeID = 6543;
```

#### Delete
This command will always affect an entire row.
```
DELETE FROM Product
WHERE ProductID = 12345;
```
Or multiple rows:
```
DELETE FROM Product
WHERE ProductID IN (12345, 12346, 12349);
```
==**USE WITH CAUTION! THIS WILL DELETE ALL ROWS WITHOUT A WHERE CLAUSE!**==

A good practice is to first write the query as a select, to ensure you are only getting the rows you want, then turn that into a delete.