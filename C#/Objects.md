A class is a data structure that contains variables and functions (methods) collectively known as "members".  If not declared as `static` it can be used to create "instances" that are assigned to a variable for use by other classes.  `static` fields or methods can be accessed without an instance, but can only interact with other `static` items.

Access to members from outside is controlled by "access specifiers" which will typically deny access to variable methods but allow access to methods that can get and set the data.  This technique of "data hiding" ensures that stored data is safely encapsulated.

An access specifier may be any one of these keywords:

- `public` - accessible from any place the class is visible
- `private` - accessible only to other members of the same class
- `protected` - accessible only to other members of the same class and members of classes derived from that class
- `internal` - accessible only to members of the same assembly

A basic example:

```
Dog fido = new Dog();

fido.setValues("Fido", 3, "Brown");

string tagF = String.Format("{0} is a {1} year old {2} dog",
    fido.getName(),
    fido.getAge(),
    fido.getColor() );

Console.WriteLine(tagF + fido.bark());

public class Dog
{
    private string name, color;
    private int age;

    public void setValues( string name, int age, string color)
    {
        this.name = name;
        this.age = age;
        this.color = color;
    }
    public string getName() { return name; }
    public int getAge() { return age; }
    public string getColor() { return color; }

    public string bark() { return "\nWoof woof!\n"; }
}
```

Naming convention is to have the class starting uppercase, and getter/setters camelCase of setVarName etc.  Top level code must precede any class declarations.

##### Constructors

A constructor is a method whose name is the same as its type.  It contains an optional access specifier, and a list of parameters.  For the Dog example above its constructor would be:

```
public Dog(string name, string color, int age)
{
    this.name = name;
    this.color = color;
    this.age = age;
}
```

They are used to create the object.  An alternative way to instantiate the object without requiring a constructor is to pass the arguments in curly braces using a key value syntax during creation as follows:
```
Dog shah = new Dog() {Name = "Shah", Color = "Gold", age = 10}
```

---
### Properties
To aid encapsulation you can set up properties as getters/setters for members in an object.  These can also help with validation.  These properties will be available for descendants of the class too.

public class Employee
{
    private string name;
    private int age;

    public string Name
    {
        get { return name; }
        set 
        {
            name = value;
        }
    }

    public int Age
    {
        get { return age; }
        set
        {
            if (value < 0)
            {
                age = 0;
            }
            else
            {
                age = value;
            }
        }
    }
}

---

### Inheritance

Classes can be created as brand new or derived from existing ones using `:` (like `extends` in other languages).  A simple example:

```
Rectangle rect = new Rectangle();
rect.setValues(4, 5);
Console.WriteLine($"Rectangle area: {rect.area()}");

public class Polygon
{
    protected int width, height;

    public void setValues(int width, int height)
    {
        this.width = width;
        this.height = height;
    }
}

public class Rectangle : Polygon
{
    public int area()
    { return width * height; }
}
```

##### Base constructors

Derived classes do not inherit the constructor method of their parent.  To call an overloaded (same name but different number of params) constructor use the `base` keyword as follows:

```
Son brad = new Son();
Son carl = new Son(100);

public class Parent
{
    public Parent()
    {
        Console.WriteLine("Parent called");
    }
    public Parent(int num)
    {
        Console.WriteLine("Parent+ called: " + num);
    }
}


public class Son : Parent
{
    public Son()
    {
        Console.WriteLine("\tSon called\n");
    }

    public Son(int num) : base(num)
    {
        Console.WriteLine("\tSon+ called: " + num);
    }
}
```

This would output:

```
Parent called
        Son called

Parent+ called: 100
        Son+ called: 100
```

---

### Hiding base methods

A method in a derived class can be declared to hide a similar method in the base class - name, return type, and parameters must all match exactly.  To indicate this is intentional the method should include the `new` keyword

---

### Polymorphism - allowing modification of base methods

Base class methods can include the `virtual` keyword to indicate it can be overwritten in derivative classes by the `override` keyword.  The original can still be called with `base.originalMethod()` but by default the most specific one will be used based on the object called.

```
Pidgeon joey = new Pidgeon();
Chicken lola = new Chicken();

describe(joey);
describe(lola);

static void describe( Bird obj )
{
    obj.talk();
    obj.fly();
}

public class Bird
{
    public virtual void talk() { Console.WriteLine("A bird talks..."); }
    public virtual void fly() { Console.WriteLine("A bird flies...\n"); }
}

public class Chicken : Bird
{
    public override void talk() { Console.WriteLine("A chicken says: \"Cluck Cluck!\""); }
    public override void fly() 
    {
        Console.WriteLine("A chicken can't fly...");
        base.fly();
    }
}

public class Pidgeon : Bird
{
    public override void talk() { Console.WriteLine("A pidgeon says: \"Coo coo!\""); }
    public override void fly() 
    { 
        Console.WriteLine("A pidgeon flies away...");
        base.fly();
    }
}
```

##### Capability classes

Classes whose sole purpose is to be derived from are called "Capability classes" - they provide capabilities to the derived classes.  They generally contain no data, but declare a number of methods to be overridden.

A capability class and all its methods can be decared using the `abstract` keyword to indicate its sole purpose is to provide a framework.  Abstract methods should be followed by a `;` instead of `{ statements }`

`sealed` is a safeguard that blocks classes being used as a base class.  Obviously this cannot be added to an abstract class.

This example is a more robust way to replicate the above example using these:

```
Pidgeon joey = new Pidgeon();
Chicken lola = new Chicken();

describe(joey);
describe(lola);

static void describe(Bird obj)
{
    obj.talk();
    obj.fly();
}

public abstract class Bird
{
    public abstract void talk();
    public abstract void fly();
}

public sealed class Pidgeon : Bird
{
    public override void talk() { Console.WriteLine("A Pidgeon says: \"Coo coo!\""); }
    public override void fly() { Console.WriteLine("A pidgeon flies away...\n"); }
}

public sealed class Chicken : Bird
{
    public override void talk() { Console.WriteLine("A Chicken says: \"Cluck cluck!\""); }
    public override void fly() { Console.WriteLine("A chicken can't fly...\n"); }
}
```

---

### Employing partial classes

Class definitions can be spread across multiple files by including the `partial` keyword in each separate part of the definition.  Add a class to the project with `Alt + Shift + C` (or `Project -> Add Class`) 