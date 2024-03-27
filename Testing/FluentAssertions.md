Instead of using asserts, allows you to write more fluent, chained tests, which give more verbose errors on failure.

Install FluentAssertions and add the `using FluentAssertions;` statement.

The [documentation site](https://fluentassertions.com/introduction) gives great examples of all use cases, but here are some for strings:

```
string actual = "abcdefghij";
actual.Should().Be("abcdefghij");
actual.Should().StartWith("abc").And.Contain("def").And.EndWith("ij");
```

By default, if you have multiple `Should()` calls, as soon as one fails it will stop the test.  To combat this you can encompass all checks in an `AssertionScope()` (which will require `using FluentAssertions.Execution;`) like so:

```
using (new AssertionScope())
{
    items.Should().NotBeNullOrEmpty();
    items.Should().HaveCount(4);
    items.Should().BeInAscendingOrder(x => x.ProductName);
    items.Should().OnlyHaveUniqueItems();
}
```

This will also work if the assertions are chained using `.And` - otherwise only the first to fail will give an error message.