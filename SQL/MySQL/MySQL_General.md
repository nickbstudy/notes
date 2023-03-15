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
No duplicate values allowed, can be null.