Data is stored in a hierarchy in appsettings.json, e.g.
```
"Features": {
  "HomePage": {
    "EnableGreeting": true,
    "EnableWeatherforecast": false,
    "ForecaseSectionTitle": "What's the weather doing?"
  }
},
```

Accessing appsettings.json requires injecting IConfiguration in the constructor (for all examples here it will have been named `_configuration`).  

Upon compilation of the app the data is flattened, and must be accessed like this:
```
if (_configuration.GetValue<bool>("Features:HomePage:EnableGreeting"))
{
	Greeting = _greetingService.GetRandomGreeting();
}
```

If there are multiple options in a section you need to access, rather than typing out the entire hierarchy each time you can assign a section to a var, then just reference each part:
```
var homePageFeatures = _configuration.GetSection("Features:HomePage");

if (homePageFeatures.GetValue<bool>("EnableGreeting"))
{
	Greeting = _greetingService.GetRandomGreeting();
}
```

You can retrieve values by referencing the indexer like this (all will be treated as strings by default):
```
string sectionTitle = homePageFeatures["SectionTitle"]
```

One more approach would be to create a simple class (probably private) with **exactly** the same property names as in appsettings.json, and use `Bind()`.  For example:
```
private class Features
{
	public bool EnableGreeting { get; set; }
	public bool EnableWeatherForecast { get; set; }
	public string ForecastSectionTitle { get; set; } = string.Empty;
}
```
Then where you need to access the settings, create a new object of this type, and give `Bind` 2 arguments; the area to map, and the name of the object to map to:
```
var features = new Features();
_configuration.Bind("Features:HomePage", features);
```
This makes it slightly easier to reference items"
```
// Instead of this:
if (homePageFeatures.GetValue<bool>("EnableGreeting")) { }

// You can use:
if (features.EnableGreeting) { }

// And instead of this:
var title = homePageFeatures["ForecastSectionTitle"];

// You can use:
var title = features.ForecaseSectionTutle;
```

---

## Applying the `Options` pattern

`Microsoft.Extensions.Options` will need to be used for this.

You can set this up using dependency injection and different types of IOption interfaces too.  Add a configuration folder in the root, then create a file with a public class called SomethingConfiguration.  In there, add public properties exactly matching those in appsettings.json (similar to above) with public setters.  It will also need to be registered in Program.cs (example below), Then instead of having to create an object to use these, you can inject them in the constructor wherever you are referring to them (shown in Index.cshtml.cs below):

**/Configuration/HomePageConfiguration.cs**
```
public class HomePageConfiguration
{
	public bool EnableGreeting { get; set; }
	public bool EnableWeatherForecast { get; set; }
	public string ForecastSectionTitle { get; set; } = string.Empty;
}
```

**Program.cs**
```
builder.Services.Configure<HomePageConfiguration>(builder.Configuration.GetSection("Features:HomePage"));
```

**Index.cshtml.cs**
```
private readonly HomePageConfiguration _homePageConfiguration;

public IndexModel(IOptions<HomePageConfiguration> options)
{
  _homePageConfiguration = options.Value;
}
```
This registers `IOptions<T>` as a singleton instance (persists for the lifetime of the app), you can instead declare it as an `IOptionsSnapshot<T>` to register as a scoped instance, which will provide a new version on each request, and reflect any changes in appsettings.json live - although that will fail if instantiated in a singleton.  To get around this, instead use `IOptionsMonitor<T>` which is a singleton, and instead of using `.Value` uses `.CurrentValue` to fetch an updated config when required.

`IOptionsMonitor<T>` also has an OnChange which takes a task. It won't reload the page, but can be used for other things such as updating information and/or logging.
```
options.OnChange(config =>
{
	_greetingConfiguration = config;
	logger.LogInformation("The greeting color changed");
});
```

---

#### Using named options

This is not supported by IOptions, you must use IOptionsSnapshot or IOptionsMonitor.

Useful when you have similar items, and the options are grouped together in appsettings.json - e.g.
```
"ExternalServices": {
  "WeatherApi": {
    "Url": "http://localhost:5001",
    "MinsToCache": 10
  },
  "ProductsApi": {
    "Url": "http://localhost:5002",
    "MinsToCache": 1
  }
}
```
An overload of the `Configure` method supports providing a named argument:
```
builder.Services.Configure<ExternalServicesConfiguration>("WeatherApi", builder.Configuration.GetSection("ExternalServices:WeatherApi"));
builder.Services.Configure<ExternalServicesConfiguration>("ProductsApi", builder.Configuration.GetSection("ExternalServices:ProductsApi"));
```
To consume the named options, inject into the constructor:
```
public WeatherApiClient(
    HttpClient httpClient, 
    IOptionsMonitor<ExternalServicesConfiguration> options,
    ILogger<WeatherApiClient> logger)
```
This does not need to be mapped to a readonly property like other dependencies.  Simply access it by creating an instance with:
```
var externalServicesConfig = options.Get("WeatherApi");
```
Then access its properties like so:
```
var url = externalServicesConfig.Url;
```
To improve this further, to prevent having string all over the place referring to the names (which can be prone to errors, and difficult if you change a name somewhere), you can introduce a constant string in another class, and refer to that instead:
```
public class ExternalServicesConfiguration
{
	public const string WeatherApi = "WeatherApi";
	public const string ProductsApi = "ProductsApi";
}
```
Then refactor Program.cs:
```
builder.Services.Configure<ExternalServicesConfiguration>(ExternalServicesConfiguration.WeatherApi, builder.Configuration.GetSection("ExternalServices:WeatherApi"));
builder.Services.Configure<ExternalServicesConfiguration>(ExternalServicesConfiguration.ProductsApi, builder.Configuration.GetSection("ExternalServices:ProductsApi"));
```
And your consuming class:
