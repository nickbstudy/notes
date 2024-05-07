Disabling tracking can improve performance if you know the data you are using won't be altered.  You may want to have a separate context for this, a good example would be something with a web frontend that will only be viewing data.

For a single query: 
```
var thing = _context.Things.AsNoTracking().FirstOrDefault();
```

On the context:
```
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
    .UseSqlServer(connectionStringGoesHere)
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking)       
}
```
↑ This can be overridden to enable tracking on queries similarly to the first way of disabling by using `AsTracking()`
