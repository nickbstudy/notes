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
public void sayHello() {
    System.out.println("Hello");
}
```
