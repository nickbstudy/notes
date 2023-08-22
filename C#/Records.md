***Records cannot be used in Entity Framework**
Used instead of a class in certain circumstances.  The constructor is typically contained in the object declaration containing its properties (which are immutable), and followed by a semicolon.  These are required when creating an instance of the object.
```
record Customer(string FirstName, string LastName);

// to create:
Customer customer = new("Nick", "Brooker");
```
The record adds functionality over a class such as:

#### Immutability

All fields are immutable by default (possible to override by creating a manual setter, but this should be avoided).

A similar of a record can be created to just change desired values with the `with` keyword:
```
var first = new Customer("Nick", "Brooker");
var second =  first with { FirstName = "John" };
```
The result will be a new instance

#### Value based equality

You can compare two records with `.Equals` or `==` and if all fields match it will return true (unlike a class, which is only a pointer).  This will only work if they are of the same type (inheritance will break this, as type equality is checked first).

#### ToString improved

Rather than class.ToString only printing its name, calling `ToString()` on a record will output the name and all of its properties

#### Built in deconstructor

Can be deconstructed without manually creating a deconstructor (gives values in the order they are defined)  Note that writing custom override constructors other than the original one will break this and it will need to be manually defined.

---

### Extending the functionality

Instead of simply defining the record name and properties then ending with `;` you can write it like a class and add more members.  Be sure to only create new properties with `init;` instead of `set;`

You can also add members just like in a class, but remember that records should be used to represent data, rather than business logic (which should live in a class).