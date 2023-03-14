### DCL - Data Control Language

This only has 2 keywords: `GRANT` and `REVOKE` and is used for setting permissions on a database.

---

#### Grant

```
CREATE USER 'frank'@'localhost' IDENTIFIED BY 'pass';
```
Then:
```
GRANT type_of_permission ON database_name.table.name TO 'username'@'localhost'
```

```
GRANT SELECT ON sales.customers TO 'frank'@'localhost';

GRANT ALL ON Sales.* to 'frank'@'localhost';
```

---
#### Revoke

Opposite of `GRANT`

```
REVOKE SELECT ON sales.customers FROM 'frank'@'localhost';
```