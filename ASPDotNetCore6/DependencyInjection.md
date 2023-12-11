#### Shortcut for extracting an interface

Right click the class name, choose 'Quick actions and refactorings' (or just `Ctrl + .`) and select "Extract interface..."

---

#### Registering services for injection

In program.cs:

To simplify adding services, add the line `var services = builder.Services` for direct access.  This is where we will register services.

Services can generally be added in any order.  The exception to the is when intentionally registering multiple implementations of the same abstraction **clarification needed**

```
var services = builder.Services;

services.AddTransient<IWeatherForecaster, RandomWeatherForecaster>();
```

They can also be registered without an interface, simply using:
```
services.AddTransient<WhateverService>();
```
---

#### Service lifetimes

- **Transient** - A new service is created for every single request.  Thread safe, good if state is involved (won't collide with other instances).  Slight performance hit on more garbage collection, but in small-mid sized apps this is negligible.  Good default choice.

- **Singleton** - One shared instance for the lifetime of the container (application).  Generally more performant, good for types with expensive or time consuming work at creation.  **Must be thread safe - avoid mutable state.**  Best for functional stateless services & caches

- **Scoped** - Sits between the above, an instance of a scoped service lives for the length of the scope from which it's resolved.