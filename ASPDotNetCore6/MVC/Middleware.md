Look into implementing IMiddleware

```
public class SomeNewMiddleware : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        // don't execute if a query is present
        if (context.Request.QueryString.HasValue)
        {
            return;
        }
        
        // do middle stuff

        await next.Invoke(context);
    }
}
```

Program.cs:
```
builder.Services.AddTransient<SomeNewMiddleware>();
...
app.UseMiddleware<SomeNewMiddleware>();
```

Can also use a factory pattern with `IMiddlewareFactory`?