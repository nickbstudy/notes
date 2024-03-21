#### Mocking definition:  
Replacing the actual dependency that would be used at production time, with a test-time-only version to enable easier isolation of the code we want to test.

There are 4 different types of test doubles:

- **Fakes** - Provide a working implementation of the dependency.  Not suitable for production (e.g. EF Core in-memory provider)
- **Dummies** - Passed around, but never used/accessed.  Used to satisfy parameters where null isn't an option.
- **Stubs** - Can provide answers to calls (could be prop gets, or method return values)
- **Mocks** - Allow you to expect/verify calls to properties or methods.

Moq allows us to create Dummies, Stubs, and Mocks.

To create a simple mock dummy to pass as an argument where something is required (`mockThing` being whatever name you want to assign)

```
Mock<IYourInterfaceName> mockThing = new Mock<IYourInterfaceName>();
var sut = new SomethingEvaluator(mockThing.Object)
```

**Note the `.Object` above - that is required to reference an object**


##### MockBehavior
Mocks can be either `Loose` or `Strict` (they are loose by default).  Loose mocks will return the default of value types, a null for reference types, and empty arrays or enumerables.  Strict mocks will throw and exception if a mocked method is called but has not been setup
```
Mock<IMockThing> mockThing = new Mock<IMockThing>(MockBehavior.Strict)
```

**Loose benefits:**
Fewer lines of setup code
Setup only what's relevant
Less brittle tests
Existing test continue to work if things change

**Strict benefits:**
More lines of setup code
May have to setup irrelevant things
Have to setup each called method
More brittle tests
Existing tests may break if things change

---

#### Mocking return values (parameter matching)

After the Mock has been created, call the `Setup` method and either provide a lambda or use one of the `It` expressions to give a scenario, then the `.Returns()` to provide a result.  For example, to check if the `IsValid` method is provided `"test"` then return `true` :
```
mockThing.Setup(x => x.IsValid("test")).Returns(false)
```
Rather than specifying an explicit value, you can use the built in parameter matching methods:
- `It.IsAny<T>` - matches anything of the given type `T`
- `It.Is<string>(yourVar.StartsWith("a"))` - to allow for a predicate (a function returning a bool), obviously string and startswith can be changed as required
- `It.IsInRange<string>("a", "z", Moq.Range.Inclusive)` - Allows anything in range (`Inclusive` can be swapped for `Exclusive`, which would only accept b-y).  `<string>` can be removed to make use of type inference too.
- `It.IsIn("a", "b", "c")` - Accepts an IEnumerable or a params array of what will be matched
- `It.IsRegex("[a-z]")` - Checks if the regex matches

If you need to have multiple calls in the function, instead use `SetupSequence` then chain on returns as required:
```
mockThing.Setup(x => x.IsAny<string>)
    .Returns(false)
    .Returns(true);
```

---

#### Mocking `out` variables

Declare a variable of the type required before the `Setup` method, then add it as a parameter:
```
bool isValid = false
mockThing.Setup(x => x.MethodName(out isValid));
```

---

#### Mocking properties

Similar to configuring methods, provide a labmda for the property name and a return value.

`mockThing.Setup(x => x.PropertyNameHere).Returns("THEMOCK");`

This can also be a local function (useful if you need to get data from somewhere else)
```
[Fact]
{
    var mockThing = new Mock<ThingToMock>
    mockThing.Setup(x => x.PropName).Returns(ExternalThing);

    var sut = new ThingImTesting(mockThing.Object);

    var application = new CreditCardApplication { Money = 20000 };

    CreditCardApplicationDecision decision = sut.Evaluate(application);

    Assert.Equal(CredicCard)
}

string ExternalThing() 
{
    // e.g. read from external file
    return "Result";
}
```

**Specifying default behavior for Loose Mocks**

If you need to mock an reference type with something other than null (only permissible for interfaces, abstract classes, or non-sealed classes) use:
```
mockThing.DefaultValue = DefaultValue.Mock
```

**Tracking changes to Mock Property Values**

By default, changes made to mocked properties will not be retained.  If you need a single one to be retained use
```
mockThing.SetupProperty(x => x.PropToRetainHere)
```
Or to retain all changes to all properties use `mockThing.SetupAllProperties();`

Note that this should be done before any individual property assignments, or it will cause a null pointer exception

---

#### State vs Behavior testing

State testing is looking at properties and values, whereas behavior testing is for checking if a method was run or a property was accessed.  

#### Behavior testing:

**Verifying a method was called**

Calling the method `mockThing.Verify(x => x.MethodName("params"));` will only allow the test to pass if the mock was called with the specified parameters (or omit for none).  You can also use Is.Any<T> etc instead of specific parameters.  Custom error messages can be added with an overload of a string of the error message you want.

You can check how many times it was called with an overload struct `Times` with options such as `Once` `Never` and `Exactly(42)`

**Verifying a Property Getter or Setter was called**

Similar pattern to above, but we use `mockThing.VerifyGet(x => x.PropertyNameHere)` also with struct `Times` overload.

To check for a setter call use `mockThing.VerifySet(x => x.Prop = "new value");` - also works with `It.Is<T>` etc as the new value, and the `Times` overload.

Finally, a method of `mockThing.VerifyNoOtherCalls()` exists to ensure nothing else gets invoked.

---

#### Other Moq techniques

**Throwing Exceptions from Mocks**

Use `mockThing.Setup` but instead of `.Returns` use `.Throws<Exception>();` (or a more specific exception if required).  You can include a message with `.Throws(new Exception("Message here"));`

**Checking a Mock was called multiple times with different values**

Create a list to capture to, `Setup` the mock to use `Capture.In(yourList)` then `Assert.Equal()` with that.

```
Mock<ILetters> mockThing = new Mock<ILetters>

var thingsToCheck = new List<string>();
mockThing.Setup(x => x.IsValid(Capture.In(thingsToCheck)));

var sut = new SystemUnderTestThingee(mockThing.Object);

sut.AddStuff("aa");
sut.AddStuff("bb");
sut.AddStuff("cc");

Assert.Equal(new List<string> {"aa", "bb", "cc"}, thingsToCheck)
```

**Mocking members of concrete types with partial mocks**

If you need to mock a class without an interface, simply make it virtual.

**LINQ Style formatting**

You can also declare using a LINQ style of formatting, for example instead of
```
Mock<IFrequentFlyerNumberValidator> mockValidator = new Mock<IFrequentFlyerNumberValidator>();
mockValidator.Setup(x => x.ServiceInformation.License.LicenseKey).Returns("OK");
mockValidator.Setup(x => x.IsValid(It.IsAny<string>())).Returns(true);
```
you can use:
```
IFrequentFlyerNumberValidator mockValidator = Mock.Of<IFrequentFlyerNumberValidator>
    (
        validitor => 
        validitor.ServiceInformation.License.LicenseKey == "OK" &&
        validitor.IsValid(It.IsAny<string>()) == true
    );
```

**Utilizing the constructor & DRY coding**

If many tests are using the same object, as with other fields you can declare them as private then set them up in a constructor:
```
private Mock<IMockThing> mockThing;
private SomeEvaluatorThing sut;

public NameOfYourTestSuite()  // constructor
{
    mockThing = new Mock<IMockThing>();
    mockThing.SetupAllProperties;
    mockThing.Setup(x => x.Info.License.LicenseKey).Returns("P2Y4C-2TBK9");
    mockThing.Setup(x => x.IsValid(It.IsAny<string>())).Returns(true);
}

[Fact]
// etc...
```

**Mocking Async method return values**

To mock the following interfaces with async such as:
```
public interface IDemoInterfaceAsync
{
    Task StartAsync();
    Task<int> StopAsync;
}
```

Set up like this:

```
var mock = new Mock<IDemoInterfaceAsync>();
mock.setup(x => x => x.StartAsync()).Returns(Task.CompletedTask);

// or for the call with a return value:
mock.Setup(x => x.StopAsync()).Returns(Task.FromResult(42));
// or a shorter way of writing this:
mock.Setup(x => x.StopAsync()).ReturnsAsync(42);
```
