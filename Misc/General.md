### Finding a connection string in SSMS

In VS: Open SQL Server Object Explorer > Right-click database > Properties > under General will be a "Connection string" box containing it. 

### SOLID:

The SOLID principles are a set of five design principles intended to make software designs more understandable, flexible, and maintainable. They are widely regarded as best practices in object-oriented programming. Here's a brief summary of each:

- #### Single Responsibility Principle (SRP):
    - ****Concept:**** A class should have only one reason to change, meaning it should have only one job or responsibility.
    - ****Benefit:**** This principle makes classes easier to understand and maintain, and reduces the impact of changes.


- #### Open/Closed Principle (OCP):
    - **Concept:** Software entities (like classes, modules, functions, etc.) should be open for extension but closed for modification.
    - **Benefit:** This principle allows for adding new functionality without changing existing code, which can help prevent introducing new bugs.

- #### Liskov Substitution Principle (LSP):
    - **Concept:** Objects of a superclass should be able to be replaced with objects of its subclasses without affecting the correctness of the program.
    - **Benefit:** This principle ensures that a subclass can stand in for its superclass, leading to more robust and flexible code, especially when dealing with inheritance hierarchies.

- #### Interface Segregation Principle (ISP):
    - **Concept:** Clients should not be forced to depend on interfaces they do not use. This often involves splitting large interfaces into more specific ones.
    - **Benefit:** This principle helps in creating more fine-grained interfaces that cater to specific client requirements, thus making the system more decoupled and easier to refactor, change, and redeploy.

- #### Dependency Inversion Principle (DIP):
    - **Concept:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions.
    - **Benefit:** This principle leads to a decoupling of software modules, making them more reusable and their interdependencies easier to manage.