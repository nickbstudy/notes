### TCL - Transmission Control Language

Requests of `INSERT`, `DELETE` and `UPDATE` must end with `COMMIT` to save the changes to the database:
```
UPDATE customers
SET last_name = 'Johnson'
WHERE customer_id = 4
COMMIT;
```

---

`ROLLBACK` can undo changes