You can create a deconstructor for objects, interfaces, classes, structs, or records by writing a method called `Deconstruct` with at least 2 parameters preceded by `out`.  These can be values or calculations, or even the result of an additional method call.  Simple example:

```
public class Order
{
    public decimal Total { get; set; } = 10.50m;
    public bool IsReadyToShip { get; set; } = true;
    public ShippingProvider? ShippingProvider { get; set; }
    public Order(decimal total, bool isReady) 
    {
        Total = total;
        IsReadyToShip = isReady;
    }

    public void Deconstruct(out decimal total, out bool ready)
    {
        total = Total;
        ready = IsReadyToShip && ShippingProvider is not null;
    }
}

public enum ShippingProvider
{
    One,
    Two, 
    Three
}
```
Then in the main program:
```

Order order = new Order(12.34m, true);
order.ShippingProvider = ShippingProvider.One;

var (total, isReady) = order;

Console.WriteLine($"{total} {isReady}");
// ↑ output is "12.34 True"
```

Not this is not a tuple, it is two variables being output to

You can have multiple deconstructors too, they all need different numbers of parameters to output.

You can also discard values you don't require when deconstructing by replacing a destination value with `_` 