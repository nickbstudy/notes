#MySQL

Keywords (CREATE, ALTER, etc) cannot be variable names.  Nothing is case-sensitive.  Statements must end with `;`

### DDL - Data Definition Language

- CREATE
- ALTER
- DROP
- RENAME
- TRUNCATE

### DML - Data Manipulation Language

- SELECT
- INSERT
- UPDATE
- DELETE

### DCL - Data Control Language

- GRANT
- REVOKE

### TCL - Transmission Control Language

- COMMIT
- ROLLBACK

---
#### Primary Keys
A column (or set of columns) whose value exists (not null) and is unique for every record in a table.  Each table can have only one primary key - typically the ID#.  Not all tables will have a primary key.

#### Foreign Keys
Defines the relationship between tables - commonly referencing a primary key (though not required).

##### Unique Keys
No duplicate values allowed, can be null.  Automatically become an index, so when removing use `ALTER TABLE table_name DROP INDEX unique_key_field;`

---

### Referring to a database

Either set a default with `USE database_name` or refer to it with `db_name.table_name`

---

### DEFAULT constraint

Add the keyword at the end of a column definition:  
`number_of_complaints INT DEFAULT 0`  
Or to alter an existing one:
```
ALTER TABLE customers
CHANGE COLUMN number_of_complaints number_of_complaints INT DEFAULT 0;
```

### NOT NULL constraint

Add `NOT NULL` to the end of a column to ensure it contains data (or `NULL` to allow it).  Alter syntax is:
```
ALTER TABLE companies
MODIFY company_name VARCHAR(255) NOT NULL
```

---

**Comments** can be done 3 ways:
```
/* Everything between these
is commented out */
# Another way
-- And another
```

---

