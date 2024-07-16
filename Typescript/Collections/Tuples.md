Each element in the tuple is strongly typed.  Functions similarly to an array, so consider using the `readonly` keyword if you want to lock it down.

Declaring: `let myFirstTuple: [string, number] = ["I'm a string", 100];`

Defining: `type myTupleType [string | null, number, object?];` - note the ? to show this value may be absent, and | null for string to allow nulls

Tuples can be nested (referring to other tuple types).

To get all values out of a tuple:
`let [gameName, gameYear, gamePlatform] = game; //where game is a tuple containing all of those values - types will be inferred.`
You can also omit parts of a tuple to get just the values you want.