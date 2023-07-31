# SOLID

---

### Single Responsibility Principle

Identify 'responsibilities' and 'jobs'.  Watch out for reasons to change.  Favour multiple small classes over fewer, more complex ones.

---

### Open Closed Principle

You should be able to extend a class's behavior without modifying it.  Separate 'bedrock' code into base classes.  Implement specializations on top of that.  Watch for classes with multiple switch/if blocks, and break those into smaller classes

---

###  Liskov Substitution Principle

Derived types must be substitutable with their base types

---

###  Interface Segregation Principle

"Clients (consumers) should not be forced to depend upon interfaces that they do not use."  Create interfaces for a class's abilities.  Group related functionality together.  Keep interfaces cohesive.

---

###  Dependency Inversion Principle

"High level modules should not depend on low level modules.  Both should depend of abstractions".