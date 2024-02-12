Most common types of tests:

- **Unit Tests** - Should have low complexity, be fast, and well encapsulated.
- **Integration Tests** - Checks whether or not two or more components work together correctly.  Medium complexity, relatively slow, not well encapsulated.  Used to check if changes result in failure.
- **Functional (end-to-end) Tests** - Test the full req/res cycle of an application.  High complexity, slow, badly encapsulated.

(MS TestHost & TestServer sometimes used?)

NSubstitute - good option instead of Moq?

---

#### Decoupling dependencies

Following the Single Responsibility Principle will help avoid this.  Use dependency injection instead of hard-coded implementations.  A good example would be a processor method which also outputs its results directly.  Instead it should output them for an injected handler method to output.  This not only better follows SOLID, but allows testing by using a fake handler.
```
public class MachineDataProcessorTests
{
    [Fact]
    public void ShouldSaveCountPerCoffeeType()
    {
        // Arrange
        var coffeeCountStore = new FakeCoffeeCountStore();
        var machineDataProcessor = new MachineDataProcessor(coffeeCountStore);
        var items = new[]
        {
            new MachineDataItem("Cappuccino", new DateTime(2022,10,27,8,0,0)),
            new MachineDataItem("Cappuccino", new DateTime(2022,10,27,9,0,0)),
            new MachineDataItem("Espresso", new DateTime(2022,10,27,10,0,0))
        };

        // Act
        machineDataProcessor.ProcessItems(items);

        // Assert
        Assert.Equal(2, coffeeCountStore.SavedItems.Count);

        var item = coffeeCountStore.SavedItems[0];
        Assert.Equal("Cappuccino", item.CoffeeType);
        Assert.Equal(2, item.Count);

        item = coffeeCountStore.SavedItems[1];
        Assert.Equal("Espresso", item.CoffeeType);
        Assert.Equal(1, item.Count);
    }
}

public class FakeCoffeeCountStore : ICoffeeCountStore
{
    public List<CoffeeCountItem> SavedItems { get; } = new();
    public void Save(CoffeeCountItem item)
    {
        SavedItems.Add(item);
    }
}
```
---
#### Test Driven Development

Break the code down into tiny requirements, and write a failing test for them.  Write the code to make it pass, refactor/improve it if possible, then carry on writing tests then code.  Typically done one test/requirement at a time.

Advantages of this:

- Thinking about what the code should do, rather than just how you're going to do it.
- Getting fast feedback from the test
- You will automatically create modular, testable code.
- Everything will be maintainable - you will know instantly if you have broken something.