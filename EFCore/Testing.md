In the DbContext file, create a parameterless constructor as well as the one to take DbContextOptions<T>, and in the OnConfiguring add a check to only populate it if `!optionsBuilder.IsConfigured`.

An example of the context file would be:

```
public class PubContext : DbContext
{
    public DbSet<Author> Authors { get; set; }
    public DbSet<Book> Books { get; set; }

    public PubContext() { }

    public PubContext(DbContextOptions<PubContext> options) : base(options) { }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            optionsBuilder.UseSqlServer(
          "connectionStringGoesHere"
        ).LogTo(Console.WriteLine,
                new[] { DbLoggerCategory.Database.Command.Name },
                LogLevel.Information)
        .EnableSensitiveDataLogging();
        }

        // OnModelCreating method if required...
    }
```
