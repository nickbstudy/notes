Instead of using asserts, allows you to write more fluent, chained tests, which give more verbose errors on failure.

Install FluentAssertions and add the `using FluentAssertions;` statement.

---

#### Strings

```
string actual = "abcdefghij";
actual.Should().Be("abcdefghij");
actual.Should().StartWith("abc").And.Contain("def").And.EndWith("ij");
```
