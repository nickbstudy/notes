A class is a template for creating object, providing state storage (properties) and behavior (methods), and encapsulate reusable functionality.

#### Constructors

```
class ReferenceItem {
    
    constructor(title: string, publisher?: string) {

    }
}
```

#### Properties & Methods

```
class ReferenceItem {
    numOfPages: number;   // standard property

    get editor(): string {
        // custom getter logic goes here, needs a return value
    }
    set editor(newEditor: string) {
        // custom setter logic, must contain exactly 1 argument
    }

    printChapterTitle(chapterNum: number): void {
        // method to print title here
    }
}
```

#### Parameter properties for constructor

You can declare properties public in the constructor to avoid having to declare them separately:

```
class Author {
    constructor(public name: string) { }
}
```

#### Static properties
They can be added to any object to be globally accessible, rather than per instance:
```
class Library {
    constructor(public name: string) { }
    static description: string = "A font of knowledge";
}
let lib = new Library('Auckland Library');
let name = lib.name;  // available from the instance
let desc = Library.description;  // available from the class
```

#### Access Modifiers
By default all classes are public (you can add the keyword but it's not required).  You can add a private modifier if required.

---

An example of all so far:

```
class Video {

    private _producer: string = '';
    static medium: string = 'Audio/Visual';

    get producer(): string {
        return this._producer.toUpperCase();
    }
    set producer(newProducer: string) {
        this._producer = newProducer;
    }

    constructor(public title: string, private year: number) {
        console.log('Creating a new Video...');
    }

    printItem(): void {
        console.log(`${this.title} was released in ${this.year}`);
        console.log(`Medium: ${Video.medium}`);
    }
}

let vid: Video = new Video('El Camino', 2019);
vid.printItem();
vid.producer = 'Vince Gilligan';
console.log(vid.producer);
```

---

#### Extending a class
Allows a class to share its member definitions with a subclass.  Must have a constructor calling the `super()` function to invoke its parent constructor.  Remember that private fields wont be visible, but protected will.  Full example of an implementation:

```
class Video {

    private _producer: string = '';
    static medium: string = 'Audio/Visual';

    get producer(): string {
        return this._producer.toUpperCase();
    }
    set producer(newProducer: string) {
        this._producer = newProducer;
    }

    constructor(public title: string, protected year: number) {
        console.log('Creating a new Video...');
    }

    printItem(): void {
        console.log(`${this.title} was released in ${this.year}`);
        console.log(`Medium: ${Video.medium}`);
    }
}

class Documentary extends Video {

    constructor(newTitle: string, newYear: number, public subject: string) {
        super(newTitle, newYear)
    }

    printItem(): void {
        super.printItem();
        console.log(`Subject: ${this.subject} (${this.year})`);
    }
}

let vid = new Documentary('The History Of Stuff', 2000, 'film history');
vid.printItem();
```

#### Abstract Classes

Similar to an interface, but they contain an implementation.  Basically a class that can't be instantiated.  You can mark methods `abstract` to force a unique implementation by children.  A full example:

```
abstract class Video {

    private _producer: string = '';
    static medium: string = 'Audio/Visual';

    get producer(): string {
        return this._producer.toUpperCase();
    }
    set producer(newProducer: string) {
        this._producer = newProducer;
    }

    constructor(public title: string, protected year: number) {
        console.log('Creating a new Video...');
    }

    printItem(): void {
        console.log(`${this.title} was released in ${this.year}`);
        console.log(`Medium: ${Video.medium}`);
    }

    abstract printCredits(): void;
}

class Documentary extends Video {

    constructor(newTitle: string, newYear: number, public subject: string) {
        super(newTitle, newYear)
    }

    printItem(): void {
        super.printItem();
        console.log(`Subject: ${this.subject} (${this.year})`);
    }

    printCredits(): void {
        console.log(`Producer: ${this.producer}`);
    }
}

let vid: Video = new Documentary('The History Of Stuff', 2000, 'film history'); 
// This being a type of Video is OK because its type inherits from it but doesn't directly implement it
vid.producer = 'Vince Gilligan';
vid.printItem();
vid.printCredits();
```