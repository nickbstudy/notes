- `public` - This can be accessed by any other code in the same assembly or another assembly that references it
- `private` - This can only be accessed by code in the same class or struct
- `protected` - This can only be acessed by code in the same class or struct, or in a derived class
- `private protected` (added in C# 7.2) - This can be accessed by code in the same class or struct, or in a derived class from the same assembly, but not from another assembly
- `internal` - This can be accessed by any code in the same assembly, but not code in another assembly
- `protected internal` This can be accessed by any code in the same assembly, or by any derived class in another assembly.

Assembly = DLL or EXE

[Good image representation](https://raw.githubusercontent.com/gist/michail-peterlis/67ab9f81f16cd2fb074d8ea9c8008653/raw/1b41929acf1cab64b4cb386966659e079a9edef5/access_modifier.svg)

---

##### Static #####

The `static` modifier on a class means that it can not be instantiated, and that all of its members are static.  A static member has one version regardless of how many instances of its enclosing type are created.  In other words, you can't use the `new` keyword to create a variable of the class type.  Its members must be accessed by using the class name itself.
