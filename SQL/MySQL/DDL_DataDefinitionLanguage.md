### DML - Data Definition Language:

DDL is the part of the SQL syntax that allows us to set up a table

---
The `CREATE` statement is used for creating entire databases and database objects as tables:

```
CREATE object_type object_name;

CREATE TABLE object_name(column_name data_type);
```

Table names can coincide with the name assigned to the database.

---
The `ALTER` statement is used when altering existing objects, with `ADD` `REMOVE` `RENAME`:

```
ALTER TABLE sales
ADD COLUMN column_name TYPE;
```
---
the `DROP` statement will delete an entire table.

---
`RENAME` syntax: 
```
i.e. RENAME object_type object_name TO new_object_name

RENAME TABLE customers TO customer_data;
```
---
`TRUNCATE` will clear all contents without deleting the table: 
```
i.e. TRUNCATE object_type object_name

TRUNCATE TABLE customers
```