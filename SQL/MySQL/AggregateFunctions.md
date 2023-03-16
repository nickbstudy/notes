#Aggregate Functions

Applied on multiple rows of a single column of a table and return an output of a single value.  They will ignore null values unless told not to.

- `COUNT()` usage must have no white-space before parentheses:
`SELECT COUNT(column_name or * for all rows) FROM table_name;`
To get unique values use `COUNT(DISTINCT column_name)`
- `SUM()`
- `MIN()`
- `MAX()`
- `AVG()`

### HAVING
If you need to have conditions in an aggregate function don't use `WHERE` for them, they must go after `GROUP BY` and before `ORDER BY`
```
SELECT 
    *, AVG(salary)
FROM
    salaries
GROUP BY emp_no
HAVING AVG(salary) > 120000;
```