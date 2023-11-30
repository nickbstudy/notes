These work similarly to partials

Create a folder in Root and Views/Shared called Components.  

**To write the logic:**
Create a file in /Components called `WhateverViewComponent.cs"
Declare a and have it implement `ViewComponent`
Use a constructor with dependency injection for any required repositories and/or logger
Create a method as follows:
```
public Task<IViewComponentResult> InvokeAsync()  // you can add arguments if required
{
    var products = _productRepository.All();

    logger.LogInformation($"Found a total of {products.Count()} products");

    return Task.FromResult<IViewComponentResult>(View(products));
}
```

Async would be required in the declaration if using async, then check the return type too (unsure exactly what that would be for async).

In /Views/Shared/Components add a folder called whatever came before ViewComponent and in there create Default.cshtml

To include this component in another view add `@await Component.InvokeAsync("NameYouGaveIt", arg1, arg2)`