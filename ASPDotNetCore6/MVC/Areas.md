Create an "Areas" folder in root, then another called whatever your new area is.  It will need its own Controllers and Views folder (don't forget to add _ViewStart.cshtml and _ViewImports.cshtml).

Just before the class declaration for each controller add `[Area("AreaNameHere")]

Links must specify `asp-area="AreaNameHere"` before the controller

Create a new route listing in Program.cs - an example for an area called Admin
```
app.MapAreaControllerRoute(
    name: "admin",
    areaName: "Admin",
    pattern: "Admin/{controller=Dashboard}/{action=Index}/{id?}");
```
Then all the controller headers would be `[Area("Admin")]`