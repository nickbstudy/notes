### Arrange → Act → Assert

Isolate and test small components (typically methods) by validating values and outcomes.

Add a test project (xUnit is good - basically just includes the xUnit NuGet package), then add a project reference to the target.

Unit tests are just like methods.

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