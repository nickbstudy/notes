#### Closures

Functions with private variables/functions only accessible from within, similar, which expose only certain functions.

#### IIFE

Immediately Invoked Function Expression - Wrap a function in parentheses then add `()` after to make it available.  For example:
```
const calculator = (() => {
    const add = (a, b) => a + b;
    const sub = (a, b) => a - b;
    const mul = (a, b) => a * b;
    const div = (a, b) => a / b;
    return { add, sub, mul, div };
  })();

  calculator.add(3, 8)   // returns 8
  ```