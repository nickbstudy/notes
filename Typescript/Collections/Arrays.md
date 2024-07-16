#### General notes

Most arguments will take a negative number to indicate elements before the last one.

#### Creation:

Any of these will work:

```
let anyArray: Array<any> = [];
let arrayObjects: object[] = []
let arrayStrings: Array<string> = new Array();
let arrayStrings: Array<string> = Array();
```

Just be consistent.

#### Adding/removing elements

```
let addRemove: number[] = [3, 6];

addRemove.push(9); // adds to the end
addRemove.unshift(0); // adds to the start

addRemove.pop(); // removes the last value
addRemove.shift(); // removes the first

let popRemoved: number | undefined = addRemove.pop(); // saves the popped value - undefined is required as it may not be present

// addRemove.splice(index, elements to delete, ...elements to add)

```

#### Copying an array

= will just copy the reference, use either `concat()` `splice()` with no args, or a spread operator in `[]` like `let newArray = [...oldArray]`

#### Sorting and reversing

`.sort()` and `.reverse()` - doesn't make a copy, uses same referenced array

#### Searching

`let foundAt: number = arrayName.indexOf('thing');` - if value is -1 there are none.  To search again, capture the index and add an argument of the last result + 1
`let existsIn: boolean = arrayName.includes('thing');`

#### Map and Filter
Map will alter each element of the array, filter will return only those that satisfy a given check:
```
let myArray: string[] = ["i'm", "You're", "We're"];
let filteredArray: string[] = myArray.filter(m => m.length > 3)
let alteredArray: string[] = myArray.map(m => m + " funny!")
```

#### Checking if array type

`Array.isArray(variableName)` evaluates to a boolean

#### Looping through an array

