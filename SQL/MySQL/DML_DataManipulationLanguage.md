### DML - Data Manipulation Language

The `SELECT` statement is used to define what is returned. Getting everything would be `SELECT * FROM sales`

---

The `INSERT` statement is used to insert data into tables, using the `INTO` and `VALUES` keywords.  You will have to specify the column names you want to enter data into unless you are filling all columns.

Specifying names:
```
INSERT INTO sales (purchase_number, date_of_purchase) VALUES (1, '2022-10-11');
```
Otherwise:
```
INSERT INTO sales VALUES (1, '2022-10-11');
```
---
The `UPDATE` statement is used as follows:
```
UPDATE sales
SET date_of_purchase = '2022-12-12'
WHERE purchase_number = 1;
```
---

The `DELETE` statement will remove all records from a table unless conditions are given with `WHERE`:
```
DELETE FROM sales
WHERE
    purchase_number = 1;