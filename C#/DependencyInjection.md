If you are using a WinForm or console app you need to first install the NuGet package Microsoft.Extensions.Hosting (3rd party alternatives are Autofac and Ninject).  

In Program.cs create a host builder interface, and register the services 

---

#### For a console app

1. Install `Microsoft.Extensions.DependencyInjection`

2. Set up interfaces and implementations:
```
public interface ITestService
{
    void Execute();
}

public class TestService : ITestService
{
    public void Execute()
    {
        Console.WriteLine("Service Executed");
    }
}

```
3. Configure Services:
```
static void Main(string[] args)
{
    // Create a new ServiceCollection
    var services = new ServiceCollection();
    
    // Configure your services with the DI container
    services.AddTransient<ITestService, TestService>();
    
    // Build the ServiceProvider
    var serviceProvider = services.BuildServiceProvider();

    // If required use the ServiceProvider to get instances of your services
    var testService = serviceProvider.GetService<ITestService>();
    testService.Execute();
}
```

4. Implementing Dependency Injection:
Now that the services are configured, you can inject them into the constructor:
```
private readonly ITestService _testService;

    public Consumer(ITestService testService)
    {
        _testService = testService;
    }
```