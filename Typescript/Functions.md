Giving a return type and arguments some type annotations will add clarity and help intellisense:
`function goodFunc(score: number, message: string): string { }`

If you want to enforce this behavior use `--noImplicityAny` in tsconfig

#### Optional parameters

`function CreateCustomer(name: string, age?: number);` optionals need to be after required.

#### Rest parameters

Argument option to make an array out of any number of arguments (must be the last function):
`function AllNumbers(...nums: number[]): void { }

#### Lambdas

`let SquareIt = x => x * x;`
`let Adder = (a, b) => a + b;`
`let Greeting = () => console.log('Hello World');`

Often uses inferred type annotations for return types.

#### Function Overloads

JS doesn't natively support this, so you have to first declare each possible function, then use any and check what type was given to evaluate the result.
```
function GetTitles(author: string): string[];
function GetTitles(available: boolean): string[];
function GetTitles(bookProperty: any): string[] {
    if(typeOf bookProperty == 'string') {
        // get by author and add to foundTitles
    }
    else if(typeOf bookProperty == 'boolean') {
        // get by availability instead
    }

    return foundTitles
}
```

Another example that behaves differently with number of arguments:

```
function GetTitles(director: string): string[];
function GetTitles(director: string, streaming: boolean): string[];
function GetTitles(director: string, streaming?: boolean): string[] {
  const allMovies = GetAllMovies();
  const searchResults: string[] = [];

  if(streaming !== undefined) {
    for(let movie of allMovies) {
      if(movie.director === director && movie.streaming === streaming) {
        searchResults.push(movie.title);
      }
    }
  } else {
    for(let movie of allMovies) {
      if(movie.director === director) {
        searchResults.push(movie.title);
      }
    }
  }
  return searchResults;    
}
```

#### Function Types

You can define a function as a variable type then use it as a function.  The syntax to declare it is:
```
let IdGenerator: (chars: string, nums: number) => string;
```
where chars and nums are arguments, and string is the return type, then implement it from another function with matching types or just a lambda:
```
IdGenerator = (name: string, id: number) => `name: ${name}, id: ${id}`;

let newID: string = IdGenerator('jedi', 123);
console.log(newID);
```