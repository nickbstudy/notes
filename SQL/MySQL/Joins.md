# Joins

### Inner Joins

Returns all records who have a common value in the column named in both tables (think middle of a Venn diagram).  Data type must be identical, name can be different (but should be the same for clarity).  First item of `SELECT` should be what you are matching on.  Table names are shortened to a letter for easier reference, as they have to be fully typed many times otherwise:
```
SELECT 
    e.emp_no, e.first_name, e.last_name, m.dept_no, e.hire_date
FROM
    employees e
        JOIN
    dept_manager_dup m ON e.emp_no = m.emp_no;
```
If you might have duplicate values, use `GROUP BY` on the field containing them and they will be unique

Can be typed `INNER JOIN` or `JOIN`

### Left Join
This is not restricted to values shared between tables.  Will return all records from the first mentioned table (so make sure that is the table referenced in the first select column).  Can also be called with `LEFT OUTER JOIN` but that is less common.

### Right Join
Just like above but returns from latter table.  Seldom applied in practice.

### Cross Join
Generates the 'Cartesian Product' of given tables; that is, a combination of all possible records.  Useful if there is no common data between tables.
![Cross Join example](https://www.sqlshack.com/wp-content/uploads/2020/02/sql-cross-join-working-principle-1.png)

### Joining multiple tables
Just continue adding JOIN statements - but be careful, if you are not very clear you can end up with a massive result.
```
SELECT 
    e.first_name,
    e.last_name,
    t.title,
    m.from_date,
    d.dept_name
FROM
    employees e
        JOIN
    dept_manager m ON m.emp_no = e.emp_no
        JOIN
    departments d ON d.dept_no = m.dept_no
        JOIN
    titles t ON t.emp_no = e.emp_no;
```