Implementing the interface `IEnumerable` will require you to have a `GetEnumerator()` method.  This must provide the item at the current index.  If you are working with a list, you can do it an easy way and return the lists `.GetEnumerator()` instead.  For example:

```csharp
using System.Collections;

class HeroAcademy : IEnumerable
{
    public string Name { get; set; } = "Olympus";
    private List<string> _olympians = new List<string>()
    {
        "Zeus", "Poseidon", "Aphrodite"
    };

    public IEnumerator GetEnumerator()
    {
        return _olympians.GetEnumerator();
    }
}
```

If you are writing a function to loop an unknown number of times, for a performance increase implement an `IEnumerable<>` and use `yield return` (operates like a return but doesn't stop the function) as follows:

```
IEnumerable<int> GetNumbers()
{
    for (int i = 0; i < 10; i++)
    {
        yield return i;
    }
}
```

If a query only wants `GetNumbers().First()` it will stop after the first run.

---

The `IEnumerator` interface has 3 members: 

- `public object Current` returns the current object being iterated over
- `public bool MoveNext` should increase the index (which needs to start at -1) and returns true until it reaches the end, then returns false
- `public void Reset` will reset the index to -1

It is quite simple to implement a custom iterator.  One extending on the above example:

```
using System.Collections;

class HeroAcademy : IEnumerable
{
    public string Name { get; set; } = "Olympus";
    private List<string> _olympians = new List<string>()
    {
        "Zeus", "Poseidon", "Aphrodite"
    };

    public IEnumerator GetEnumerator()
    {
        return new AcademyEnumerator(_olympians);
    }
}

class AcademyEnumerator : IEnumerator
{
    private List<string> _names;
    private int _index = -1;

    public AcademyEnumerator(List<string> names)
    {
        _names = names;
    }
    public object Current => _names[_index];

    public bool MoveNext()
    {
        _index++;
        return _index < _names.Count;
    }

    public void Reset()
    {
        _index = -1;
    }
}
```

Then to use it from `Program.cs`

```
var academy = new HeroAcademy();

foreach (var hero in academy)
{
    Console.WriteLine(hero);
}
```

---

You can take this even further by implementing generic types, which then need their subtypes to be added (good for enabling LINQ queries)