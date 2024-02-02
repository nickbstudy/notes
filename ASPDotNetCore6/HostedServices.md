'Hosted services' provide a clean pattern for executing code, within an ASP.NET Core application, outside of the request pipeline.  Each service is started as an asynchronous background Task.

Typical use cases include:
- Polling for data from an external service
- Responding to external messages or events
- Performing data-intensive work, outside of the request lifecycle

Create a class that inherits from `BackgroundService`

You will probably want a constructor with the required dependencies/options/etc.

Set up the ExecuteAsync method as a `while (!stoppingToken.IsCancellationRequested) { }`

**In Program.cs** register the service as `services.AddHostedService<NameOfYourService>();`

Here is an example of a caching service for a weather API:

```
using Microsoft.Extensions.Options;
using TennisBookings.External;

namespace TennisBookings.BackgroundServices;

public class WeatherCacheService : BackgroundService
{
	private readonly IWeatherApiClient _weatherApiClient;
	private readonly IDistributedCache<WeatherResult> _cache;
	private readonly ILogger<WeatherCacheService> _logger;

	private readonly int _minutesToCache;
	private readonly int _refreshIntervalInSeconds;

	public WeatherCacheService(
		IWeatherApiClient weatherApiClient,
		IDistributedCache<WeatherResult> cache,
		IOptionsMonitor<ExternalServicesConfiguration> options,
		ILogger<WeatherCacheService> logger)
	{
		_weatherApiClient = weatherApiClient;
		_cache = cache;
		_logger = logger;
		_minutesToCache = options.Get(ExternalServicesConfiguration.WeatherApi).MinsToCache;
		_refreshIntervalInSeconds = _minutesToCache > 1 ? (_minutesToCache - 1) * 60 : 30;
	}

	protected override async Task ExecuteAsync(CancellationToken stoppingToken)
	{
		while (!stoppingToken.IsCancellationRequested)
		{
			var forecast = await _weatherApiClient
				.GetWeatherForecastAsync("Eastbourne", stoppingToken);

			if (forecast is not null)
			{
				var currentWeather = new WeatherResult
				{
					City = "Eastbourne",
					Weather = forecast.Weather
				};

				var cacheKey = $"current_weather_{DateTime.UtcNow:yyyy_MM_dd}";

				_logger.LogInformation("Updating weather in cache.");

				await _cache.SetAsync(cacheKey, currentWeather, _minutesToCache);
			}

			await Task.Delay(TimeSpan.FromSeconds(_refreshIntervalInSeconds),
				stoppingToken);
		}
	}
}

```