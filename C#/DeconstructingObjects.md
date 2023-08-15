You can create a deconstructor for objects, interfaces, classes, structs, or records by writing a method called `Deconstruct` with at least 2 parameters preceded by `out`.  These can be values or calculations, or even the result of an additional method call.  Simple example:

```
public void Deconstruct(out decimal total, out bool ready)
{
    total = Total;
    ready = IsReadyToShip && ShippingProvider is not null;
}
```