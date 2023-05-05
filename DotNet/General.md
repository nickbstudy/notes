
# .NET

General tips

---

`dotnet watch` will start a local server refreshing with each save

**Inserting C# code** - small amounts of code can be added to the .razor file by prefixing with @ such as variables and functions: `<h3>Welcome to @placeName</h3>` or larger blocks after the html as such:
```
@code {
    /// more code goes here
    /// multi line stuff etc
}
```
You can have as many of these code blocks as you need.

**Binding inputs with variables** - inside the `@code { }` block you declare variables for use, bind an input to them with `@bind="yourVarName"` as a param