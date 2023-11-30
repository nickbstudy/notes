#### Attribute based routing

In the controller you can define an address (differing from the method name if you wish), and arguments can be passed in the route and assigned using attributes with:
```
[Route("/details/{productId}/{somethingElse?}")]
public IActionResult ProductDetails(int productId, string? somethingElse) { }
```
Note that the argument names must match the input.

To apply a type constraint to the route, add `:type` to the parameter, e.g. `{productId:int}` - this will give a 404 if you attempt to reach the page without the appropriate structure.

These are some of the built-in route constraints:

- `{age:int}` - any integer
- `{birthday:datetime}` - a valid DateTime in the invariant culture: 2000-12-31
- `{title:minlength(10)}` - the string must be at least 10 characters long
- `{age:range(18, 120)}` - any integer from 18 to, and including 120
- `{name:alpha}` - case insensitive alphabetical characters

You can also use regex for this: `{something:regex(^[[a-zA-Z0-9 ]]+$)}` (note the double square brackets are required for this)

#### Routing in Program.cs

Alternatively, you can create a new `app.MapControllerRoute` to achieve this:
```
app.MapControllerRoute(
    name: "detailsRoute",
    defaults: new { action = "ProductDetails", controller = "Products" },
    pattern: "/details/{productId}/{somethingElse?}");
```

---

#### Custom constraints

Implementing `IRouteConstraint` (which only requires a bool `Match` to return) allows creation of custom routing rules.  Create a folder in root called "Constraints" and implement something like the below:

```
public class NameConstraint : IRouteConstraint
{
    public bool Match(HttpContext? httpContext, 
        IRouter? route, 
        string routeKey,
        RouteValueDictionary values, 
        RouteDirection routeDirection)
    {
        // try to extract the "name" part of the URL, return false if it fails
        if(!values.TryGetValue("name", out var name))
        {
            return false;
        }

        // if successful create a clean string
        var nameAsString = Convert.ToString(name,
            CultureInfo.InvariantCulture);

        if(string.IsNullOrWhiteSpace(slugAsString))
        {
            return false;
        }

        // return true if regex check passes - this will apply to parameter generation
        // as well, so if you are transforming you need to account for that
        return Regex.IsMatch(slugAsString, "^[a-zA-Z]+$");

    }
}
```

This needs to be added in Program.cs as follows:
```
builder.Services.addRouting(options => {
    options.ConstraintMap["validateName"] = typeof(NameConstraint);
});
```
Then you can use it with the specified name, e.g. `[Route("/something/name:validateName")]`

This is a poor example, as you could simply use the `:alpha` to replace this, but it demonstrates the steps involved.

---

#### Parameter Transformation

If you need to create a route path from a string with unusual characters you can do this by creating an instance of `IOutboundParameterTransformer` and incluing that in the routing options.  It is suggested to keep this in a root folder called "Transformations".  A typical example is:
```
public class TrailParameterTransformer : IOutboundParameterTransformer
{
    public string? TransformOutbound(object? value)
    {
        if(value is not string)
        {
            return null;
        }

        return Regex.Replace(value.ToString()!, @"[^a-zA-Z0-9]+", "-",
            RegexOptions.CultureInvariant,
            TimeSpan.FromMilliseconds(200)).ToLowerInvariant().Trim('-');
            // timespan is to prevent attacks.
    }
}
```
Then add the option:
```
builder.Services.AddRouting(options => {
    options.ConstraintMap["validateName"] = typeof(NameConstraint);
    options.ConstraintMap["trailTransform"] = typeof(TrailParameterTransformer);
});
```