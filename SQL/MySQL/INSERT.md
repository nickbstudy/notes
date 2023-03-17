### Inserting into a table

This contains two parts.  If you are filling in all columns you don't need to specify them, otherwise you need to include which ones you are going to provide values for.

```
INSERT INTO table_name (
    column_name
    another_column
    random_one
)
VALUES (
    'First',
    'Second',
    'Last'
);
```

Otherwise just `INSERT INTO table VALUES ('Val1', 'Val2');`

You can copy data using a query, as above naming any columns or omitting if all are to be used:
```
INSERT INTO departments_dup
SELECT * FROM departments;
```