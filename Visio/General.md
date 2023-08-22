### Global unhandled exception handling

To cover any unhandled exceptions in console, at the top of Program.cs add

```
AppDomain currentAppDomain = AppDomain.CurrentDomain;
currentAppDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleException);

static void HandleException(object sender, UnhandledExceptionEventArgs e)
{
    Console.WriteLine($"Sorry there was a problem and we need to close.  Details: {e.ExceptionObject}");
}
```

note the method was not static by default, had to set that manually.

---

### Create and use custom exceptions

Only use these in exceptional circumstances.  .NET probably has something built in, look for that first.  Don't use them in regular logic flow, they should only be for irregularities.  If you are throwing one from another caught exception consider wrapping that in it.  They can be useful for interacting with APIs as well when you know what errors you are likely to get.

The standard naming convention is `SomethingDescriptiveException`

Inherit from `Exception` (or your own custom exceptions)

An example of custom exceptions:

```
public class CalculationException : Exception
{
    private const string DefaultMessage = "An error occured during the calculation, make sure the operation is supported and the values are good.";

    public CalculationException() : base(DefaultMessage) { }

    public CalculationException(string message) : base(message) { }

    public CalculationException(Exception innerException) : base(DefaultMessage, innerException) { }

    public CalculationException(string message, Exception innerException) : base(message, innerException) { }
}

```