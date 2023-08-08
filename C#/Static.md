

In classes, the `static` modifier means that rather than making a copy when an object is created, the static member is shared between all other instances of that class.  It is also available without the object being created.  Regular methods can still work with static fields, or you can create static methods (which can only interact with static fields) which must be called directly.

`static` members are only accessible through the class since it's been created at class-level.  Rather than calling an object, you invoke it with the class name: `WhateverItsCalled.YourMethod()`