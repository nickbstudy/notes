- **Candidate Key** - Unique, but not a primary key.  E.g. Email, SSN, ISBN
```
employee_ssn char(9),
constraint const_employee_ssn unique (employee_ssn)
```
