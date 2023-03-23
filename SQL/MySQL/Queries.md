### Operator types for `WHERE`

- `AND`
- `OR`
- `IN` - returns all within `()`, better performance.  Usage: `...WHERE first_name IN ('Denis', 'Elvis');`
- `NOT IN` - returns all except things named in `()`
- `LIKE` - contains a given pattern. % indicates any sequence - `...WHERE first_name LIKE('Mar%');` would return Mark, Martin, Margaret etc. `_` substitutes a single letter.
- `NOT LIKE` will not contain thing given in `()`
- `BETWEEN ... AND ...` given values will be included in query
- `NOT BETWEEN ... AND ...` excludes given values, won't include given variables
- `IS NOT NULL` / `IS NULL` pretty obvious.
- Mathematics operators: `= > >= < <= <>` or not equal `<>` or `!=`

---

### `ORDER BY`

`ORDER BY column_name, next_column;` and you can add `ASC` or `DESC`

---

### `GROUP BY`

Commonly combined with aggregate functions, syntax is:
```
SELECT column_name(s)
FROM table_name
WHERE conditions
GROUP BY column_name(s)
ORDER BY column_name(s)
```
It is also good practice to select the column you have a result from e.g.
```
SELECT 
    first_name, COUNT(first_name)
FROM
    employees
GROUP BY first_name;
```

---
### Aliases

Used to rename a selection from your query with the keyword `AS` e.g.
`SELECT first_name, COUNT(first_name) AS names_count` 
Just delivers clearer output.

---
### `LIMIT`
Final line of a query should be `LIMIT 10;` or however many records you want returned.  Keep in mind ordering, as this will affect which ones you include.

---
### `IFNULL()`
To replace null fields with something:
```
SELECT 
    dept_no, IFNULL(dept_name, 'Dept not provided') AS dept_name
FROM
    departments;
```
Using the alias is not required but looks much better

---
### `COALESCE()`

Given multiple arguments, returns the first non-null. Would return `Test` from the below query:
`SELECT COALESCE(NULL, NULL, NULL, 'Test', NULL, 'Example');`

---
### `UNION` and `UNION ALL`

`UNION ALL` is used to combine a few `SELECT` statements in a single output - allowing you to unify tables.  There must be the same number of columns in each, and have the same name/order/data type - if this would be difficult you can add 'null columns' (`NULL AS column_name`).  
`UNION` functions the same but ignores duplicates. Syntax is:  
```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

---
### Subqueries
Also called 'Inner queries' or 'Nested queries', these are used to narrow down returned data by using other tables.  The inner query must be surrounded by parentheses.  This can be in `WHERE` with `IN` OR `EXISTS`, both of which can be prefixed with `NOT`.  `IN` is bettter for small data sets and `EXISTS` is for larger ones.  SQL will process the inner query first, then use that in comparing the outer ones.  Syntax for `IN` is:
```
SELECT * FROM employees e
WHERE e.emp_no IN (
    SELECT m.emp_no
    FROM dept_manager m
);
```
And an example of `EXISTS`:
```
SELECT * FROM employees e
WHERE EXISTS (
	SELECT * FROM titles t
	WHERE t.emp_no = e.emp_no
	AND title = 'Assistant Engineer'
);
```
Subqueries can also be used in `SELECT` or `FROM`