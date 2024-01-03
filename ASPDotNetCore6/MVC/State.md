You can use cookies, sessions, tempdata, or other request details such as query parameters and form posts to track state.  It is shared between tabs, but not browsers.

Add the `Microsoft.AspNetCore.Session` NuGet package if it is not already present.

The first example uses an in-memory cache.  It is the most performant, but doesn't scale well with high use applications.

In Program.cs:

```
builder.Services.AddHttpContextAccessor();
builder.Services.AddDistributedMemoryCache();  // for the memory cache store
builder.Services.AddSession();

...
app.UseSession();  // should be between UseRouting and MapRazorPages
```

You can configure options for the session if required like so:
```
builder.Services.AddSession(options => 
{
    options.Cookie.HttpOnly = true;  // prevents xss
    options.Cookie.Name = "Whatever";  // handy if you have more than 1 site in the domain
    options.IdleTimeout = TimeSpan.FromMinutes(20);  // 60 at most, 20 should be fine
    options.Cookie.IsEssential = true;  // gives warning if this is essential?
});
```
After Program.cs is configured the simplest way to read/write is:

```
HttpContext.Session.SetString("Message", "Hello");  // case-sensitive
HttpContext.Session.SetInt32("Age", 35);

var greeting = HttpContext.Session.GetString("Message");
var age = HttpContext.Session.GetInt32("Age")

HttpContext.Session.Remove("Greeting");  // deletes the key
HttpContext.Session.Clear();  // clears all keys
```
If string/int are not sufficient, and you need to work with other data types/objects you can serialize them to a **JSON string** and store than, then get it and deserialize at the endpoint - e.g.
```
HttpContext.Session.SetString("UserData", JsonSerializer.Serialize(inputData));
// then to unpack it:

Data userData = new()
var value = HttpContext.Session.GetString("UserData");
userData = JsonDeserializer.Deserialize<UserData>(value);

```



#### You can write an interface and class to simplify injection for pages as follows:

Add to Program.cs:
```
builder.Services.AddTransient<IStateRepository, SessionStateRepository>();
```

Create an interface:
```
public interface IStateRepository
{
    public void SetValue(string key, string value);
    public string GetValue(string key);
    public void Remove(string key);
}
```
Create a SessionStateRepository:
```
public class SessionStateRepository : IStateRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public SessionStateRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public string GetValue(string key)
    {
        return httpContextAccessor.HttpContext?.Session?.GetString(key) 
            ?? string.Empty;
    }

    public void SetValue(string key, string value)
    {
        httpContextAccessor.HttpContext?.Session?.SetString(key, value);
    }

    public void Remove(string key)
    {
        httpContextAccessor.HttpContext?.Session?.Remove(key);
    }
}
```
Now inject this in controllers which need to access the state, or into views using `@inject IStateRepository stateRepository`

---

#### Cookies

Below is an example of writing a cookie with optional options (Secure means it will require HTTPS, HttpOnly prevents JS reading it)
```
HttpContext.Response.Cookies.Append("someId", someId.ToString(), 
    new CookieOptions { Secure = true, HttpOnly = true});
```
To read the cookie using the HttpContext and Request data:
```
Guid.TryParse(HttpContext.Request.Cookies["someId"], out Guid someId);
```
This will persist after the browser is closed, whereas sessions are only active while the browser is open.

---

#### SQL Server sessions
More resilient than in-memory, but obviously requires a DB call so less performant.
Requires the `Microsoft.Extensions.Caching.SqlServer` NuGet package.
```
builder.Services.AddDistributedSqlServerCache(options => 
{
    options.ConnectionString = builder.Configuration.GetConnectionString("DefaultConnectionString");
    options.SchemaName = "dbo";
    options.TableName = "SessionState";
});
```

---

## *Fancy Strongly-typed Session Data Manager*

A robust state system - populate SessionVar with the types you need to enable state for.

**Program.cs**
```
builder.Services.AddDistributedMemoryCache();
builder.Services.AddSession();
builder.Services.AddHttpContextAccessor();
```

**SessionManager.cs**
```
using System.Text.Json

public class SessionManager
{
    public static HttpContext HttpContext => new HttpContextAccessor().HttpContext;

    public static void Set(string key, object value)
    {
        HttpContext.Session.SetString(key, JsonSerializer.Serialize(value));
    }

    public static T Get<T>(string key)
    {
        var value = HttpContext.Session.GetString(key);
        return value == null ? default : JsonSerializer.Deserialize<T>(value);
    }
}
```
**SessionVar.cs**
```
public class SessionVar : SessionManager
{
    public static string FirstName
    {
        get => Get<string>("FirstName");
        set => Set("FirstName", value);
    }

    public static UserData UserDataValues
    {
        get => Get<UserData>("UserDataValues");
        set => Set("UserDataValues", value);
    }
}
```

**Usage**
Using the above example, to set the `FirstName` state variable:
```
[HttpPost]
public IActionResult EnterName(string name)
{
    SessionVar.FirstName = name;
    // etc...
}
```
Then to retrieve it:
```
var nameDetail = SessionVar.FirstName;
```