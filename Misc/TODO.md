- Learn more markup with `mermaid`

```mermaid
classDiagram
    class Person {
        +Id : Guid
        +FirstName : string
        +LastName : string
        -privateProperty : string
        #ProtectedProperty : string
        ~InternalMember : string
    }

graph LR
    A-->B
    A-->C
    B-->C


```

For a flowchart: 
```
graph LR
    A-->B
```

For a class diagram:
```
classDiagram
    class Person {
        +Id : Guid
        +FirstName : string
    }

```

#### Convention for specifying visibility in class designs:

`+` Public members
`-` Private members
`#` Protected members
`~` Internal members
