Maps are unique key/value pairs

Creation: `let myMap = new Map<string, number>();` or any 2 types,
Creation with data: an array of arrays
```
let myMap = new Map<string, number>([
    [ 'Dota 2', 95 ],
    [ 'Tetris', 80 ],
    [ 'Pong', 45 ]
]);
```

Adding: `myMap.set('Diablo', 88);`
Getting: `console.log(myMap.get('Diablo')); // prints 88`
Has? bool: `myMap.has('Diablo');`
Deleting (bool result): `myMap.delete('Diablo');`
Size? number result: `myMap.size`
Clear all entries: `myMap.clear()`


