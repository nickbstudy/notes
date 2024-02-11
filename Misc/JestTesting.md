###  Test Driven Development (using Jest)

Installing Jest: `npm i --save-dev jest`

Syntax for a test in Jest (can have multiple `expect` statements):

```
// first word can be it or test
it('Test name', () => {
    expect(addingFunctionExample(1, 2, 3)).toBe(6)
})
```
Don't forget to import required functions with `const funcName = require('./file')` or create dummy functions to avoid unnecessary calls.

If you want to test the value of an object use `toEqual`:
```
test('object equality', () => {
    const data = {one: 1}
    data['two'] = 2;
    expect(data).toEqual({one: 1, two: 2})
})
```
`toEqual` ignores keys with undefined properties and object type mismatches, to take these into account use `toStrictEqual`

You can set Jest to auto-run after save by adding a watch script in `package.json`:
```
...
"scripts": {
    "test": "jest",
    "watch": "jest --watch *.js"
},
...
```
`npm test` will run once, `npm run watch` will run after every save.

You can add 'to-do' tests by having a line of just `it('Thing to test')`, and it will show as skipped in the tests.