MVC = Model View Controller.  

**Model:**
- Domain data & logic to manage data
- Simple API
- Hides details of managing the data

**View**

Razor page (C#/HTML) incorporating tag helpers (such as `asp-something`)

**Controller**

Controllers contain IActionResult methods (or more specific implementations, ViewResult, FileResult, JsonResult for APIs, RedirectResult, or many others) to handle HTTP verb requests (usually GET & POST).  If they are async (generally preferred for DB communications) then they must be passed as a Task<IActionResult>.  They can accept arguments which will be populated first from the model or the route (check order?), e.g. a link of `<a asp-controller="ContName" asp-action="DoIt" asp-route-yourArg="Goes here">` will fill the `string yourArg` argument.

Typically you want to design them as lean as possible, outsourcing query manipulation & caching etc to a repository which is brought into the constructor with dependency injection.

---

#### EF Core Migrations

When a model changes or is added/removed and the database needs to be updated to reflect that, a migration must be run.  Go to package manager and run `add-migration WhateverIsHappening` then `update-database` - the first command prepares the migration file and the second executes it.

---

## Validation

#### Validation Attributes

- `[Required]`
- `[StringLength(50)]`
- `[DataType(DataType.EmailAddress)]`
- `[Range(1, 100), ErrorMessage = "The value must be between 1 and 100."]`
- Regular Expressions - e.g. to only allow digits: `[RegularExpression(@"^\d+$", ErrorMessage = "Only digits are allowed.")]`
- `[BindNever]` - While not a validation attribute, when used on a model this indicates it is only there for utility, and should not be bound to in POST.

`[ValidateAntiForgeryToken]` should be added to the top of the controller method to ensure data is coming from the right place too.
`var formData = HttpContext.Request.Form;` on the POST method can be helpful to examine/debug details of the data being passed in too.

#### Displaying validation errors to the user 

You can use tag helpers on the view to show validation information to the user, this can be in the form of `<span asp-validation-for="PropertyName" class="text-danger"></span>` below each input, or for a summary you can include `<div asp-validation-summary="All" class="text-danger"></div>`

#### Checking everything in the controller

```
try
{
    if (ModelState.IsValid)
    {
        await _categoryRepository.AddCategoryAsync(category);
        return RedirectToAction(nameof(Index));
    }
}
catch (Exception ex)
{
    ModelState.AddModelError("", $"Adding the category failed, please try again! Error: {ex.Message}");
}
```

---

#### Misc. tips

- **Avoiding 'overposting'** - In the `[HttpPost]` method on the controller, if you only need certain values instead of sending the whole view model, you can define it as: `public async Task<IActionResult> Add([Bind("Name,Description,DateAdded")] YourItem yourItem) { }`


- **'Are you sure?'** - for delete buttons typically, on the submit button add the attribute `onclick="javascript: return confirm('Are you sure?');"`

- **ViewBag vs ViewData** - These perform similar tasks; passing small bits of information (values or objects) from the controller to the view.  There is no back and forth (use a model/vm for that).  The difference is that ViewData uses a Dictionary structure (key/value pairs), e.g. `ViewData["Message"] = "Text here"` whereas ViewBag stores its values as objects, e.g. `ViewBag.Message = "Text here"`