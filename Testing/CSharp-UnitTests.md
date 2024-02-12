### Arrange → Act → Assert

Isolate and test small components (typically methods) by validating values and outcomes.

Add a test project (xUnit is good - basically just includes the xUnit NuGet package), then add a project reference to the target.

To simplify testing you can use the same namespace by going into properties of the test project and changing the "Default Namespace" to match the main project.  This will automatically add a `<RootNamespace>` tag to represent this in the project configuration.

You can use multiple asserts in one unit test if there are many possibilities to cover, it can be better than having repetitive tests and you see an error as to why it failed anyway.

`[Fact]` is a keyword to let xUnit know a test is coming.

- **Arrange** the data - set up the object/variables to be tested
- **Act** on it - do something with a measurable outcome
- **Assert** that it is what you expect - use `Assert.Equals(something, itsComparison)` or any of the other methods of `Assert` to complete the test.  An example of two tests:

```
using HelloFromCSharp;

namespace HelloFromCSharp.Tests
{
    public class EmployeeTests
    {
        [Fact]
        public void Work_Adds_HoursWorked()
        {
            // Arrange
            Employee employee = new Employee("Bob", "Ross", 30);
            int numberOfHours = 3;

            // Act
            employee.Work(numberOfHours);

            // Assert
            Assert.Equal(numberOfHours, employee.HoursWorked);
        }

        [Fact]

        public void Work_Adds_DefaultNumber_IfNoneSpecified()
        {
            // Arrange
            Employee employee = new Employee("Bob", "Ross", 30);

            // Act
            employee.Work();

            // Assert
            Assert.Equal(1, employee.HoursWorked);
        }
    }
}
```

---
#### Data driven tests:

Include parameters in the test method, and use `[Theory]` instead of `[Fact]`.  `[InlineData]` provides the arguments passed to each sub-test:
```
[InlineData("Cappuccino", "Invalid csv line")]
[InlineData("Cappuccino;InvalidDateTime", "Invalid datetime in csv line")]
[Theory]
public void ShouldThrowExceptionForInvalidLine(string csvLine, string expectedMessagePrefix)
{
    var csvLines = new[] { csvLine };
    
    var exception = Assert.Throws<Exception>(() => CsvLineParser.Parse(csvLines));

    Assert.Equal($"{expectedMessagePrefix}: {csvLine}", exception.Message);
}
```
---
#### Run code before every test

Copy the code you need each test to run (such as if you are repeatedly making new instances of objects), create a constructor, remove the var, and Ctrl + . to make a read-only field (and optionally prefix with _).  Then obviously remove the duplicate unnecessary parts.

Old way:
```
public void ShouldSaveCountPerCoffeeType()
{
    // Arrange
    var coffeeCountStore = new FakeCoffeeCountStore();
    var machineDataProcessor = new MachineDataProcessor(coffeeCountStore);
    var items = new[]
    //etc...
```

New way:
```
private readonly FakeCoffeeCountStore _coffeeCountStore;
private readonly MachineDataProcessor _machineDataProcessor;

public MachineDataProcessorTests()
{
    _coffeeCountStore = new FakeCoffeeCountStore();
    _machineDataProcessor = new MachineDataProcessor(_coffeeCountStore);
}
[Fact]
public void ShouldSaveCountPerCoffeeType()
{
    // Arrange
    var items = new[]
    //etc...
```
---
#### Running code after every test

You can implement `IDisposable` on your test class if you need to clean up after it (delete files etc).  The `Dispose()` method you implement will run after each test in the class.

---
#### Testing console output
If an object is using Console.WriteLine for its output, you can refactor it for testability by creating 2 constructors - one with an overload for `TextWriter` and one without which by default implements `Console.Out`.  Use `TextWriter` as any other DI and then call `_textWriter.WriteLine` instead of `Console.WriteLine`.  In your tests you can then use `StringWriter` as a replacement, as it inherits from `TextWriter` 
```
private readonly TextWriter _textWriter;

public ConsoleCoffeeCountStore() : this(Console.Out) { }
public ConsoleCoffeeCountStore(TextWriter textWriter)
{
    _textWriter = textWriter;
}
public void Save(CoffeeCountItem item)
{
    var line = $"{item.CoffeeType}:{item.Count}";
    _textWriter.WriteLine(line);
}
```
And the test:
```
[Fact]
public void ShouldWriteOutputToConsole()
{
    var item = new CoffeeCountItem("Cappuccino", 5);
    var stringWriter = new StringWriter();
    var consoleCoffeeCountStore = new ConsoleCoffeeCountStore(stringWriter);

    consoleCoffeeCountStore.Save(item);

    var result = stringWriter.ToString();
    Assert.Equal($"{item.CoffeeType}:{item.Count}{Environment.NewLine}", result);
}
```