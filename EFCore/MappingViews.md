You can create a class to represent the fields of a view, then declare a DbSet of that type to include it in the context.  You also need to indicate that no key is required, and that it is a view in the `OnModelCreating` method.  The final result might be:

```
//...other DbSets
public DbSet<BooksByAuthor> { get; set; }

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<BooksByAuthor>().HasNoKey().ToView("BooksByAuthor");
}
```

You can also avoid potential typos by avoiding a string, using instead :
`ToView(nameof(BooksByArtist))`

The benefit of doing all of this is that is won't affect migrations, or try to create a table to represent the data.

Keyless entries will never be tracked.