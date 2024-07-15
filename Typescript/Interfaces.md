Interfaces don't compile to JS, they exist only to define the contract.  Duck typing applies to all entities - if it walks like a duck, swims like a duck, and quacks like a duck, then it must be a duck.  A literal code example of this is:

```
interface Duck {
    walk: () => void;
    swim: () => void;
    quack: () => void;
}
let probablyADuck = {
    walk: () => console.log('walk like a duck');
    swim: () => console.log('swim like a duck');
    quack: () => console.log('quack like a duck');
}
function FlyOverWater(bird: Duck) { }
FlyOverWater(proablyADuck);  // this works because it satisfies all the expectations of a duck
```

#### Defining an interface
```
interface Book {
    id: number;
    title: string;
    pages?: number;
    markDamaged: (reason: string) => void;  // methods need only a name and a signature
}
```
They can be used to define required properties for an object, without including any methods:

```
interface Movie {
    title: string,
    director: string,
    yearReleased: number,
    streaming: boolean
}

function GetAllMovies(): Movie[] {
    return [
      { title: 'A New Hope', director: 'George Lucas', yearReleased: 1977, streaming: true },
      { title: 'The Empire Strikes Back', director: 'Irvin Kershner', yearReleased: 1980, streaming: false },
      { title: 'Return of the Jedi', director: 'Richard Marquand', yearReleased: 1983, streaming: true },
    ];
}
```

#### Interfaces for a function type

```
interface StringGenerator {
    (chars: string, nums: number): string;
}

let IdGenrator: StringGenerator;
IdGenerator = (name: string, id: number) => `Name: ${name}, ID: ${id}`
```

#### Extending Interfaces

```
interface LibraryResource {
    catalogNumber: number;
}
interface Book {
    title: string;
}
interface Encyclopedia extends LibraryResource, Book {
    volume: number;
}

let refBook: Encyclopedia = {
    catalogNumber: 1234,
    title: 'The book of everything',
    volume: 1
}
```

#### Class types:

```
interface Librarian {
    doWork: () => void;
}
class ElementalSchoolLibrarian implements Librarian {
    doWork() {
        console.log('Reading to and teaching children...');
    }
}
let kidsLibrarian: Librarian = new ElementalSchoolLibrarian();
kidsLibrarian.doWork();
```