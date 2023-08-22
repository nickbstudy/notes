Classes can declare event handlers, which must have a protected virtual void method associated.

The class in which the event is declared is known as the 'publisher', and other places that use it are 'subscribers'.  

Event handlers are similar to delegates; they accept a function, and optionally return a value.  They should be defined with `EventHandler` or `EventHander<T>`.  A key difference however, is that delegates will accept input from anywhere with access to the class, but event handlers can only be invoked from within the publisher

A simple declaration with no event arguments:

```
public event EventHandler ThingHappened;

protected virtual void OnThingHappened()
{
    ThingHappened?.Invoke(this, EventArgs.Empty);
}
```

In order to avoid memory leaks you may need to implement IDisposable?