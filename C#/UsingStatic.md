If you are frequently using a system object, you can declare it with `using static` so you don't have to write the prefix all the time.  A good example would be a console app, you might want to start with `using static System.Console` so instead of always needing to type `Console.WhateverFunction();` you can just use `WhateverFunction();`

```
using static System.Console;

WriteLine("Enter first number");
int written = ReadLine();
```