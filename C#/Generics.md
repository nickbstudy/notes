Inside of angle brackets in a declaration you can declare anything to be a generic (all types allowed).  You can limit the types available with the `where` keyword afterwards.  An example here is creating a method which returns a list of generic items:

```
public static List<T> DoSomethingFancy<T>() {
    ...
}
```

The `T` is just a convention, it can be anything.  If you need multiple generics you can put `<T, U, V>` or whatever you want to call them.  

To construct a simple class that takes a generic data type:
```
public class GenericClass<T>
{
    public T data;
}
```

Then you can create it as anything you like with:

```
var myClass = new GenericClass<string>();
myClass.Data = "Something...";
```