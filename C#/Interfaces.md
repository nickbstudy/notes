Interfaces are a framework for objects.  They can be considered a 'contract' for what methods must be implemented.  They contain empty method declarations with no access modifiers.  A class can implement multiple interfaces by separating them with commas.  The name should begin with `I` by convention.

```
internal interface IEmployee
{
    void PerformWork();
    double ReceiveWage(bool resetHours = true);
    void PerformWork(int numberOfHours);
}
```

When instantiating an object if it implements an interface you can use that instead of the object itself - instead of
```
Manager nick = new Manager();
```
You could use
```
IEmployee nick = new Manager();
```
Useful if you are going to have them all in a list/array.