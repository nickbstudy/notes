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
[ApiController] // helps with responses etc
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
- **`[FromQuery]`** - Query string parameters - not always required as using `[ApiController]` implies string arguments are these, but for clarity you could use this as:
```
public async Task<IActionResult> GetCity([FromQuery] string? name)
```
- **`[FromRoute]`** - Route data from the current request
- **`[FromServices]`** - The service(s) injected as action parameter
- **`[AsParameters]`** - Method parameters

Typically for an API you will use body, header, query, and route.

---

#### Global Exception Handling

In Program.cs, before registering anything else in the HTTP request pipeline:
```
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler();
}
```

And in the service registration: `builder.Services.AddProblemDetails();`

This prevents sending a full stack trace, but gives a clean 500 and "something went wrong" error.  Full details will still be in the logger.

---

#### Filtering & Searching

**Filtering** a collection means limiting the collection taking into account a predicate.  For example, `/api/things?name=Test` will only return results where the "name" field matches "Test".  It allows you to be precise by adding filters until you get exactly the results you want.

**Searching** a collection means adding matching items to the collection based on a predefined set of rules.  These may be different for each case; it's up to the API to decide how to implement this functionality.  It allows you to go wider, and is used when you don't know exactly which items will be in the collection

A repository implementing both might look like:

```
public async Task<IEnumerable<City>> GetCitiesAsync(string? name, string? searchQuery)
{
    if (string.IsNullOrWhiteSpace(name) && string.IsNullOrWhiteSpace(searchQuery)) return await GetCitiesAsync();

    var collection = _context.Cities as IQueryable<City>;

    if (!string.IsNullOrWhiteSpace(name))
    {
        name = name.Trim();
        collection = collection.Where(c => c.Name == name);
    }

    if (!string.IsNullOrWhiteSpace(searchQuery))
    {
        searchQuery = searchQuery.Trim();
        collection = collection.Where(c => c.Name.Contains(searchQuery) || (c.Description != null && c.Description.Contains(searchQuery)));
    }

    return await collection.OrderBy(c => c.Name).ToListAsync();
}
```

Note that the `IQueryable<City>` is being created first for filters to be applied to, before it is run, ensuring efficient queries.


#### Paging

Limit the page size (a `const int` on the controller is fine), and return first page by default.  Paging needs to be done on the repository with deferred execution (`IQueryable`).  The controller should take `int page = 1, int pageSize = 10` (or however many you want) as arguments, it is important to add those defaults.  Implementing this in the repository is as simple as changing the return statements.  Instead of the above :

```
// without paging
return await collection.OrderBy(c => c.Name).ToListAsync();
```

You include pageNumber and pageSize in the repository and return:
```
return await collection
    .OrderBy(c => c.Name)
    .Skip(pageSize * (pageNumber - 1))
    .Take(pageSize)
    .ToListAsync();
```

Also, in the filtering example above you should remove the check for no filters to return all cities, as that would violate the max values.

---

#### Returning Pagination Metadata

It can be beneficial to the user to get an indication of the pagination results, however this doesn't belong in the JSON body response.  A good solution is to instead add it to the header.  A simple way to execute this is creating a class to hold this data:

```
public class PaginationMetadata
{
    public int TotalItemCount { get; set; }
    public int TotalPageCount { get; set; }
    public int PageSize { get; set; }
    public int CurrentPage { get; set; }

    public PaginationMetadata(int totalItemCount, int pageSize, int currentPage)
    {
        TotalItemCount = totalItemCount;
        PageSize = pageSize;
        CurrentPage = currentPage;
        TotalPageCount = (int)Math.Ceiling(totalItemCount / (double)pageSize);
    }
}
```

And instead of your repository returning a `Task<IEnumerable<Thing>>` have it return a tuple including the metadata: `Task<(IEnumerable<Thing>, PaginationMetadata)>`

Tagging on to the above examples, the final return statement would be replaced with:
```
var totalItemCount = await collection.CountAsync();

var paginationMetadata = new PaginationMetadata(totalItemCount, pageSize, pageNumber);

var collectionToReturn = await collection
    .OrderBy(c => c.Name)
    .Skip(pageSize * (pageNumber - 1))
    .Take(pageSize)
    .ToListAsync();

return (collectionToReturn, paginationMetadata);
```

Then in your controller:

```
var (cityEntities, paginationMetadata) = await _cityInfoRepository.GetCitiesAsync(name, searchQuery, pageNumber, pageSize);

Response.Headers.Add("X-Pagination", JsonSerializer.Serialize(paginationMetadata));
```

---

## <u>Security</u>

#### Token-based security


