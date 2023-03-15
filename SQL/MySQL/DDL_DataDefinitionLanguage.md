### DML - Data Definition Language:

DDL is the part of the SQL syntax that allows us to set up a table

---
The `CREATE` statement is used for creating entire databases and database objects as tables:

```
CREATE object_type object_name;

CREATE DATABASE IF NOT EXISTS Sales;

CREATE TABLE object_name(column_name data_type constraints);
```

Table names can coincide with the name assigned to the database.

Table example (including primary key and auto increment which starts at 1):
```
CREATE TABLE sales
(
    purchase_number INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    date_of_purchase DATE NOT NULL,
    customer_id INT,
    item_code VARCHAR(10) NOT NULL
);
```

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