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

Accessing appsettings.json requires injecting IConfiguration in the constructor (for all examples here it will have been named `_configuration`).  Upon compilation of the app the data is flattened, and must be accessed like this:
```
if (_configuration.GetValue<bool>("Features:HomePage:EnableGreeting"))
{
	Greeting = _greetingService.GetRandomGreeting();
}