# Classes

Classes (names should be capitalised) contain information about an object - properties and methods, and can be placed in packages (otherwise they will be in the 'default package'):

```
package model;

public class Person {
    
    private String firstName;

    public String getFirstName() {
        return firstName;
    }
}
```

Classes need constructors, getters, and setters, which can be generated automatically with: *[tbc]*
- constructor TBC
- getter TBC
- setter TBC

Classes can use "extends" to imitate another class:

```
package model;

public class Dog extends Pet {
    
    public void bark() {
        System.out.println("Ruff ruff!!!");
    }
}
```

Only public (the default) classes can be accessed from outside the package, and if in another package they must be imported with:

```
import packageName.ClassName;
```
  
    

## Methods

Methods are functions inside classes - basically where all the work gets done.

```
public class Person {
    private String middleName = "John";
    public char getMiddleInitial() {
        return middleName.charAt(0);
    }

    public int add(int num1, int num2) {
        return num1 + num2;
    }

    public static void main(String[] args) {
        Person p1 = new Person();
        System.out.println(p1.getMiddleInitial());
        System.out.println(p1.add(3, 8));
    }
}
```

Output would be: 
>J  
>11

`char` and `int` indicate return type - otherwise use `void`

You can add the `static` keyword to a method to initialize it on build, so it can be called without having to create an instance of an object.  This can be useful in 'utility classes' etc.

### Varargs

If you don't know how many args you will be receiving, put `...` after the type and it will turn all provided values into an array:

```
public void lottaNames(String... names) {
    // do something with all of them
}
```

This makes it easier to call, you can just provide values instead of an array.  If you require other args of another type they must be specified first, then varargs last.