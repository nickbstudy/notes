Razor shares most syntax with MVC, and is compatible with it.  You can have both in the same project.

To include it in a project, in Program.cs add:

- `builder.Services.AddRazorPages();` 
- `app.MapRazorPages();`

At the top of a razor page must be `@page` or to include attributes `@page "{id:int?}"`

It uses a "PageModel" system instead of controllers & views, below `@page` will be `@model SomethingPageModel` and this is where the object and its fields/methods are declared for the page to use.  It also uses a `ViewData` dictionary instead of the MVC `ViewBag` 

A model might look like:

```
public class SomethingPageModel : PageModel
{
    public Stuff? Stuff { get; private set; }

    public IActionResult OnGet() // async versions available too
    {
        Stuff = dbContext.Things.First();
        return Page();
    }

}
```

Properties which will be used in the page as a model need to be bound by having `[BindProperty]` above them, or else they will not fill the item for validation/processing in POST

### Routing

Routing using the `Pages` folder as the root, and the file name and location will represent the address.  For example:

| Route | Page file |
| ----------- | ----------- |
| / | /Pages/index.cshtml |
| /contact | /Pages/contact.cshtml | 
| /store/pies | /Pages/Store/pies.cshtml |
| /store/pies/3 | /Pages/Store/pies.cshtml  `@page "{id:int?}"` |

You can also customize the route if needed (maybe to avoid a clash with MVC) by setting `@page "/NewAddressHere"`