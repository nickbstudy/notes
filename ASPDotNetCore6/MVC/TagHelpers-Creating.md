You can create your own tag helpers for razor files by:

1. Create a folder for them in the project root called TagHelpers
2. The namespace needs to be registered - it is recommended to do this in `/Views/_ViewImports.cshtml` using this syntax: `@addTagHelper MyProject.TagHelpers.*, MyProject` 
3. Create a public class that inherits `: TagHelper` (this requires `using Microsoft.AspNetCore.Razor.TagHelpers;`)
4. The class name will define how you invoke it, e.g. `EmbiggenTagHelper` will be invoked when you write a tag of `<embiggen>`
5. Add properties for any attributes you would like to include when calling the helper.
6. `override` its `Process` or `ProcessAsync` method, which has 2 arguments (a TagHelperContext and TagHelperOutput) to assist in processing.
7. Use `output.Attributes.SetAttribute("href", "mailto:" + Address);` to set attributes manually
8. To capture the contents between tags use `var newContent = await output.GetChildContentAsync();` to capture it, then to access is `newContent.GetContent()`

Here is a new tag called "embiggen" that when given the `type="bold"` attribute returns bold, or if given none will make text italic:

```
public class EmbiggenTagHelper : TagHelper
{
    public string? Type { get; set; }

    public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
    {
        base.Process(context, output);
        if (Type == "bold")
        {
            output.TagName = "strong";

            var newContent = await output.GetChildContentAsync();
            output.Content.SetContent(newContent.GetContent());
        }
        else
        {
            output.TagName = "i";
            var newContent = await output.GetChildContentAsync();
            output.Content.SetContent(newContent.GetContent());
        }
    }
}
```

So in the razor page, this:

`<embiggen type="bold">Please contact us</embiggen> by sending an email <embiggen>using the button below</embiggen>`

Becomes:

**Please contact us** by sending an email *using the button below*