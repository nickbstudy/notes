You can use cookies, sessions, tempdata, or other request details such as query parameters and form posts to track state.

---

#### Sessions (in memory)

In Program.cs

```
builder.Services.AddHttpContextAccessor();
builder.Services.AddSession();

builder.Services.AddTransient<IStateRepository, SessionStateRepository>();

...
app.UseSession();
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