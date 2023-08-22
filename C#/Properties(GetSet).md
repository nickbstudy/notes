It is generally good practice (and better abstraction) to set a classes fields/variable as private as possible, and have public getters/setters for them.  This enables you to have only a getter/setter, or to validate on set.  Fully expanded, the simplest example is:

```
class Person
{
  private string name; // field

  public string Name   // property
  {
    get { return name; }   // get method
    set { name = value; }  // set method
  }
}
```

You can generate a getter/setter with `Ctrl + .` and encapsulate, or use the shorthand if only a simple 

An even shorter way of declaring a property, which is logically the same as above is:
```
public string Name { get; set; }
```
---
### init;
An alternative to `set;` is `init;` which will only allow setting in the constructor or when creating the object instance with {} notation, it will then be immutable.