Testing an in memory database can be done by declaring a DbContext in the test constructor like so:

```
public SomethingTest()
{
    var options = new DbContextOptionsBuilder<SomethingContext>()
        .UseInMemoryDatabase(Guid.NewGuid().ToString())
        .Options;
    var context = new SomethingContext(options)

    [Fact]
    // tests go here
}
```

Using a guid for the name of the database is an easy way to ensure you always have a fresh copy of it for each test.