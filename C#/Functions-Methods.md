Functions may be declared in Program.cs or in a class (which are then called methods).  The declaration syntax is:

```
access-type return-data-type functionName (parameters) 
{
    statements 
}
```

**`access-type`** defines visibility to other classes and can be public (globally visible), private, or static (visible only locally)

**`return-data-type`** can be any variable type, or `void` if it doesn't return anything

**`parameters`** must be have a type and can be given a default value:
```
public string mergeNames(string first, string last) { }
static void addThese(int addMe = 0, int addMeToo = 0) { }
```

There are 3 ways parameters can be passed:

- **By Value** - Arguments passed by value give a copy and do not affect the original
- **By Reference** - These assign the address of the original value to the parameter, and changes made in the function will be reflected in the original.  Reference parameters must include the `ref` keyword in the declaration and calls
- **For Output** - Arguments passed 'for output' assign the memory address to be filled by the function/method.  This must include the `out` keyword in both its declaration and call.

Variables defined within a function only have local scope, and will not conflict with names of the same type elsewhere.

---

### Overloading methods

You can have multiple methods of the same name within a class performing different functions by accepting a different number of arguments each.  The compiler recognizes the number of parameters and data types - a process called 'method resolution'