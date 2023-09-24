### Constructors

```javascript
function Book(title, author, pages, read) {
    this.title = title;
    this.author = author;
    this.pages = pages;
    this.read = read;
}

Book.prototype.info = function() {

    let readOrNot = "";
    if (this.read) {
        readOrNot = "read"
    } else {
        readOrNot = "not read yet"
    }
    return `${this.title} by ${this.author}, ${this.pages} pages, ${readOrNot}`
}

const firstBook = new Book("Harry Potter", "J K Rowling", "450", true)
console.log(firstBook.info())
```

While functions can be declared in the object constructor, this will create a new instance of it for each object created.  For small things this is not an issue but in big data it can slow things.  By adding the function to its prototype instead it will always share the one.

#### Alternative - Factory Functions

These are similar to constructors, but simply return an object when called, removing the requirement for the `new` keyword, and are considered more reliable.

```javascript
const personFactory = (name, age) => {
    const sayHello = () => console.log('Hello!')
    return { name, age, sayHello }
}

const jeff = personFactory('jeff', 27)
console.log(jeff.name)  // returns 'jeff'
jeff.sayHello()  // calls the function which logs 'Hello!'
```


---

### Prototypal inheritance

The recommended way of inheriting an object is with `Object.create`, which returns a new object with the specified properties and any additional ones you want to add.

```
function Student() {
}

Student.prototype.sayName = function() {
    console.log(this.name)
}

function EigthGrader(name) {
    this.name = name;
    this.grade = 8;
}

EigthGrader.prototype = Object.create(Student.prototype)

const bob = new EigthGrader("Bob")
bob.sayName();  // Will add "Bob" to console
```