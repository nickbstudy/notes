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

### ORDER BY

`ORDER BY column_name, next_column;` and you can add `ASC` or `DESC`

---

### GROUP BY

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

