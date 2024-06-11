Most queries include:

**SELECT** *(columns)*
**FROM** *(table)*
**WHERE** *(condition)*;

Always use single-quotes for strings.
Conditional not-equals is `<>` (though some DBMS allow `!=`)

```
SELECT Title, ReleaseYear
FROM Movie
WHERE Title <> 'Twilight';
```

Conditions can be made of several predicates (something that evaluates to true/false) using the `AND` and `OR` keywords:

`WHERE Category = 'Board Games' AND UnitPrice > 100;

Using `OR` will return results that satisfy either or both of the conditions.

If you are looking for multiple values in a column you can use `IN` (or `NOT IN`)like so:

```
SELECT *
FROM Employees
WHERE Role IN ('Manager', 'Accountant', 'Developer');
```

Don't forget about the `BETWEEN` option too.

`WHERE (price >= 100) AND (price <= 500)`
is equivalent to:
`WHERE price BETWEEN 100 AND 500

**Order of operations** - `AND` will be resolved before `OR`, so use parentheses to acheive queries.  Be liberal with them to improve clarity and ensure the query is evaluated as expected.

**Wildcard queries** - the `LIKE` keyword allows you to match on patterns.
`WHERE ProductName LIKE 'Green%';` would match 'Green Tea', 'Greenthing', etc, but nothing that doesn't start with the word 'Green'.  You can have wildcards on both sides to allow this though `'%Green%'` will match 'Big Green Vase'  It allows for any amount of text, including none.

A more specific wildcard unit is the underscore - when used with a `LIKE` query it only allows a single character in its place.  `'G_'` would match 'G3' but not 'G10'

You can use it inversly with `NOT LIKE`

Remember that `LIKE` can be somewhat more demanding - take this into consideration for large datasets.

**Checking for null** - Don't use `WHERE Something = NULL;` - this will cause an error, the correct syntax is `WHERE Something IS NULL;` or `IS NOT NULL`

**Ordering** - Last line of a query is:
`ORDER BY` RowName, NextRowName DESC;`

---

#### Aggregate functions

These are used to summarize data from multiple rows into single values.

**Count:**
```
SELECT COUNT (*)
FROM TableName
-- Optional WHERE
```

**Sum:**
```
SELECT SUM(minutes_watched)
FROM user_session
WHERE user_id = 'nick.brooker'
```

**Max (or Min, or Avg):**
```
SELECT MAX(ListPrice)
FROM Product
```

**Group By (always used with another aggregate to 'group by')**
```
SELECT COUNT(*), Category
FROM Product
GROUP BY Category;
```
**HAVING** - Uses calculated information to filter data - it will always be using an aggregate function, e.g.
```
SELECT COUNT (*), CATEGORY
FROM Product
WHERE Status <> 'Discontinued'
GROUP BY Category
HAVING COUNT(*) > 100
```
**DISTINCT** - Ignores duplicates
```
SELECT DISTINCT country
FROM customer
```
