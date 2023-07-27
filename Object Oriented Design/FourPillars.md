### Abstraction

Basically a module hiding its internal complexity, and providing a simple interface to its consumers.

Abstracting away an implementations details allows you to make changes to it more easily if a better solution is found in the future, as long as you don't alter its interface.

Resist the temptation to make everything public for expediency, consider for each item whether it should be private or protected

---

### Encapsulation

Similar to above, also suggests keeping the data as close as possible to its class.  Expose as little as possible.

---

### Inheritance

Look for commonalities to a base class, keep more specific details in separate classes and inherit from the base

---

### Polymorphism

Subtypes should be able to be used in place of their base types.  C# Only lets you inherit from a single class, but you can use multiple interfaces.  Divide up responsibilities.  Use inheritance for universal abilities, use interfaces for mix & match