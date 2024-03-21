Instead of using asserts, allows you to write more fluent, chained tests, which give more verbose errors on failure.

Install FluentAssertions and add the `using FluentAssertions;` statement.

The [documentation site](https://fluentassertions.com/introduction) gives great examples of all use cases, but here are some for strings:

```
string actual = "abcdefghij";
actual.Should().Be("abcdefghij");
actual.Should().StartWith("abc").And.Contain("def").And.EndWith("ij");
```
