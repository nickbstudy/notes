#### One to many
==is this the only inner join?==

**Constructing the query:**

`SELECT` the columns you want from both tables (for clarity you can use `TableName.ColumnName` but it isn't required)

```
FROM TableWithTheForeignKey
JOIN TableWithThePrimaryKey
ON TableWithTheForeignKey.SomethingID = TableWithThePrimaryKey.SomethingID;
```

This is an 'inner join' - it would also work if you explicitly used `INNER JOIN` instead of just `JOIN` (MS Access actually requires this).

You don't need the key in the SELECT if you don't want it returned.  A full example:

```
SELECT FirstName, LastName, Email, DateHired
FROM Employee
INNER JOIN Department
ON Employee.DepartmentID = Department.DepartmentID
```

---

#### Outer Joins - Left, Right, and Full

While an `INNER JOIN` will only return results with an exact match on a given field, if you want to include all results of a table, including null or ones which don't match any of the other values you can use one of the 3 `OUTER JOIN` options. Think of `LEFT` and `RIGHT` as including results from that side of the join.  A left outer means take everything from `Employee` in this example:
```
SELECT Name, DateHired
FROM Employee LEFT OUTER JOIN Department
ON Employee.EmployeeID = Department.DepartmentID
```
Whereas a `RIGHT OUTER JOIN` would have taken everything from `Department` instead.  A rarely used option of `FULL OUTER JOIN` exists too, taking everything from both tables.  Technically the `OUTER` keyword isn't required but it is good to include for clarity.

A shorthand way of writing this exists when a foreign key exists in a table named exactly after a primary key in another - you can omit the `ON` line and just say: `FROM Employee NATURAL JOIN Department` - however this is not supported in all DBMS (it won't work is SSMS), so the more verbose method is better.

#### Aliases

Instead of repeating the name of a table, when you define it in the `FROM` line (and after a join) add a short identifier (typically just a letter) for the table, then in the rest of the query you can use that instead of typing out the full table name.

You can also give columns an alias by adding `AS` after their name.  This can be useful for aggregate functions to improve readability, as they typically don't have a column name otherwise.