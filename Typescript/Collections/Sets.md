**Unique values only in a set**

Creation: `let myDemoSet  = new Set<string>();`
Or with data: `let myDemoSet  = new Set<string>(['Tetris', 'Pong', 'Frogger']);`

Adding data: `myDemoSet.add('Dota 2');`

Checking for data (boolean result) `myDemoSet.has('Game Name'); //false`

Deleting data (boolean true when succeeded, false otherwise)
`myDemoSet.delete('Game Name'); //false`

Count: `mydemoset.size`

Delete all: `mydemoset.Clear();`