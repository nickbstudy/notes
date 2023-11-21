In Program.cs: 

```
builder.Serivces.AddControllers();
app.MapControllers()
```
Note that these (or `AddControllersWithViews` and `MapDefaultControllerRoute`, which can be used instead) will already be present if using MVC

APIs use 'Attribute based routing' instead of 'Convention based', and inherit from `: ControllerBase` which is similar to `: Controller` but without views.  To specify routes:

```
[Route("api/[controller]")] // api is just convention, [controller] means it will point to the name
public class SomeController : ControllerBase { }
```

All HTTP verbs need to be specified, GET is not the default.  `[HttpGet]` or equivalent above the method.  Include parameters as below:
```
[HttpGet("{id}")]
public IActionResult GetById(int id) { }
```

Data returned is automatically serialized into JSON.  If objects reference other objects it can create an infinite loop and cause issues, so to avoid this add on to `AddControllers` this: 
`.AddJsonOptions(options => { options.JsonSerializerOptions.ReferenceHandler = ReferenceHandler.IgnoreCycles; });`

#### Helper Methods

ControllerBase gives some helper methods to simplify responses:

- `Ok()` 200 - successful request, add content as arguments
- `JsonResult()` 200 and forces JSON to be used
- `BadRequest()` 400?
- `NotFound()` 404
- `NoContent()` 204 - Typically sent as a response to POST when something has been done and no further communication is required.

#### POST

Accepting arguments requires an additional attribute before the parameter to indicate it will be coming from the body of the request.  E.G.
```
[HttpPost]
public IActionResult Search([FromBody] string searchQuery) {
    return _yourDbContext.Stuff.Where(i => i.Name.Contains(searchQuery));
}
```