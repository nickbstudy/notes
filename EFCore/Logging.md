#### Simple Logging

In the OnConfiguring for the context, after `.UseSqlServer(connString)` append `.LogTo()`, with the first argument being a target (category below has more info), the next being an array of the types of commands logged 

|Syntax|Description|
|---------------|---------------|
|Microsoft.EntityFrameworkCore | All EF Core messages|
|Microsoft.EntityFrameworkCore.Database | All database interactions|
|Microsoft.EntityFrameworkCore.Database.Connection | Uses of a database connection|
|Microsoft.EntityFrameworkCore.Database.Command | Uses of a database command|
|Microsoft.EntityFrameworkCore.Database.Transaction | Uses of a database transaction|
|Microsoft.EntityFrameworkCore.Update | Saving entities, excluding database interactions|
|Microsoft.EntityFrameworkCore.Model | All model and metadata interactions|
|Microsoft.EntityFrameworkCore.Model.Validation | Model validation|
|Microsoft.EntityFrameworkCore.Query | Queries, excluding database interactions|
|Microsoft.EntityFrameworkCore.Infrastructure | General events, such as context creation|
|Microsoft.EntityFrameworkCore.Scaffolding | Database reverse engineering|
|Microsoft.EntityFrameworkCore.Migrations | Migrations|
|Microsoft.EntityFrameworkCore.ChangeTracking | Change tracking interactions|

You can also filter on which level of logging (debug, information, etc) with the 3rd argument.  A call including all 3 might look like:

```
optionsBuilder
    .UseSqlServer(myDbString)
    .LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Command.Name }, LogLevel.Information);
```

---

#### Sensitive Data

Parameters for queries are protected by default.  If you need to expose them for debugging purposes append `.EnableSensitiveDataLogging()` to the configuration chain.

---

#### LogTo Targets

**Console**
 `.LogTo(Console.WriteLine)` (no `()` as it is not a method call, just a reference)

**File (use `StreamWriter`)**
A simple example would be:

```
private StreamWriter _writer = new StreamWriter("EFCoreLog.txt", append: true);
optionsBuilder.LogTo(_writer.WriteLine)
```

If you are doing this then you also need to dispose of the `_writer` after each call to ensure the file is written.  Do this by overriding the Dispose() method on the context:

```
public override void Dispose() 
{
    _writer.Dispose();
    base.Dispose();
}
```

**Debug Window**
`.LogTo(log => Debug.WriteLine(log))`