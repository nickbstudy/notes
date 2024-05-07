Delegates are a reference type used to invoke a method.  They are declared in a two-step process.  First a delegate type is declared, then instances of the delegate type are created, as shown in the following code.  A delegate type is declared using the `delegate` keyword along with the method signature of the delegate:
```
public delegate void OpDelegate(int First, int Second);
```

Any static or non-static method that conforms to the declared signature can be used as a delegate.

- Creating and using a void
- Return values and parameters
- Multicast delegates and chains ( += )
- Action & Func
Action = a void delegate
Func a delegate with a return