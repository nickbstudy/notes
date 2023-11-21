### Defining a class

Basic example of a class, constructor takes the `self` argument to give access to itself and allow instantiating fields.

`Announce` is obviously a method.

```
class Person:
    def __init__(self, idNum, name)
        self.idNum = idNum
        self.name = name
    def Announce(self)
        print(f"My name is {self.name} and my ID is {self.idNum})

somebody = Person(42, "Bob")
somebody.Announce()
```

Now inheritance - `Another` will inherit from `Something`

```
class Another(Something)
    def Repeat(self)
        print(f"{self.name} {self.name} {self.name})

