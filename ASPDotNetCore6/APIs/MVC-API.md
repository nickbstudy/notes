#### Routing

You can either map routes directly, or map them to controllers.  When creating an API mapping to controllers is preferred, as no conventions are applied:

```
app.UseRouting();

app.UseAuthorization(); // was in the slides, not explicitly required

app.UseEndpoints(endpoints => 
{
    endpoints.MapControllers();
});
```

A more modern, less verbose way of doing this is simply `app.MapControllers();`

Then in the controller, above the class declaration:
```
[ApiController]
[Route("api/cities")]
public class CitiesController : ControllerBase
{
    [HttpGet]
    public ActionResult<IEnumerable<CityDto>> GetCities()
    {
        return Ok(CitiesDataStore.Current.Cities);
    }

    [HttpGet("{id}")]
    public ActionResult<CityDto> GetCity(int id)
    {
        var cityToReturn = CitiesDataStore.Current.Cities.FirstOrDefault(c => c.Id == id);
        if (cityToReturn == null) return NotFound();
        return Ok(cityToReturn);
    }
}
```

---

#### Problem Details

To give greater detail in error messages returned, add this to the service collection
```
builder.Services.AddProblemDetails( options =>
{
    options.CustomizeProblemDetails = ctx => 
    {
        ctx.ProblemDetails.Extensions.Add("additionalInfo", "Additional info example");
    };
});
```
Now error responses will contain an additional key/value pair: `"additionalInfo": "Additional info example"`

For more fine control over which errors give which responses you can fill the ctx lambda above instead with
```
builder.Services.AddProblemDetails(options =>
{
    options.CustomizeProblemDetails = ctx =>
    {
        // Check the response status code to customize based on the range or specific codes
        if (ctx.HttpContext.Response.StatusCode >= 400 && ctx.HttpContext.Response.StatusCode < 500)
        {
            // Custom logic for 400 range errors
            ctx.ProblemDetails.Extensions.Add("errorType", "This is a client error");
        }
        else if (ctx.HttpContext.Response.StatusCode >= 500)
        {
            // Custom logic for 500 range errors
            ctx.ProblemDetails.Extensions.Add("errorType", "This is a server error");
        }
    };
});
```
For more granular control over when and how ProblemDetails are returned, you might need to implement additional middleware, use exception filters, or explicitly return ProblemDetails objects from your controllers' actions in response to specific errors or conditions. This allows for very precise control over the error handling behavior of your application.

---

#### Formatters and Content Negotiation

This is implemented by `ObjectResult`.  By default, only JSON will be used.  If an application requests anything else, it will still provide JSON unless this option is added to AddControllers:
```
builder.Services.AddControllers(options =>
{
    options.ReturnHttpNotAcceptable = true;
});
```

A very simple way to enable XML support is adding this on to the AddControllers:
```
builder.Services.AddControllers(options =>
{
    options.ReturnHttpNotAcceptable = true;
}).AddXmlDataContractSerializerFormatters();
```

---

#### Getting a File

The easiest way is just using the `File()` method in a controller as follows:
```
[HttpGet("{fileId}")]
public ActionResult GetFile(string fileId)
{
    var pathToFile = "file.txt";

    if (!System.IO.File.Exists(pathToFile))
    {
        return NotFound();
    }

    var bytes = System.IO.File.ReadAllBytes(pathToFile);

    return File(bytes, "text/plain", Path.GetFileName(pathToFile));
}
```

This will only work for txt files, as denoted in the `"text/plain"` but an easy fix is to add `builder.Services.AddSingleton<FileExtensionContentTypeProvider>();` to the service collection.  Inject it into the controller, then use it as follows:
```
if (!_fileExtensionContentTypeProvider.TryGetContentType(pathToFile, out var contentType))
{
    contentType = "application/octet-stream";  // acts as a 'catch all' for other file types
}

var bytes = System.IO.File.ReadAllBytes(pathToFile);

return File(bytes, contentType, Path.GetFileName(pathToFile));
```

---

#### Accepting Input

There are many ways to accept input to an API:
- **`[FromBody]`** - Request body
- **`[FromForm]`** - Form data in the request body
- **`[FromHeader]`** - Request header
- **`[FromQuery]`** - Query string parameters
- **`[FromRoute]`** - Route data from the current request
- **`[FromServices]`** - The service(s) injected as action parameter
- **`[AsParameters]`** - Method parameters
