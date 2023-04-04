### Test Driven Development

Syntax for a test in Jest:

```
it('Test name', () => {
    expect(addingFunctionExample(1, 2, 3)).toBe(6)
})
```
Don't forget to import required functions with `const funcName = require('./file')`

You can set it to auto-run after save by adding a watch script in `package.json`:
```
...
"scripts": {
    "test": "jest",
    "watch": "jest --watch *.js"
},
...
```
`npm test` will run once, `npm run watch` will run after every save.