**Don't forget to use parameters to avoid SQL Injection!**

`DbSet.FromSqlRaw()` and `DbSet.FromSqlInterpolated()` allow you to write your own query when the EF Core one is not suitable.  They still need an execution method (such as `ToList()`) to run, such as
```
var authors = _context.Authors.FromSqlRaw("select * from authors").ToList();
```
The expected results must be in the shape of the DbSet's entity.

---
# *Parameterizing raw SQL*

The easiest way is to use `FromSqlInterpolated` and outside variables:

```
var name = "Nick";
var person = _context.People.FromSqlInterpolated($"SELECT * FROM yourtable WHERE firstname = {name}")
```

Note the absense of `'` surrounding the search term, this is inferred by EF.  If you need to use a wildcard write it like this:

```
var start = "N";
var authors = _context.People
    .FromSqlInterpolated($"SELECT * FROM People WHERE FirstName LIKE {start} + '%'")
    .ToList();
```

---