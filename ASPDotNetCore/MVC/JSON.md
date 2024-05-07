On the controller you can specify to bind JSON received in the body to a model using the `[FromBody]` tag:

```
[HttpPost]
[Route("")]
public IActionResult ShowCart([FromBody]CartModel cartModel)
{
    return View();
}

public class CartModel
{
    public Guid? CartId { get; set; }
    public Guid? CustomerId { get; set; }
}
```

This automatically maps all fields to the relevant model.  Commonly used with AJAX, and often returns a partial view.