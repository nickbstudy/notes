Python comes with a basic set of functions called "built-in functions", but also comes with a set of modules called the "standard library".  Each module must be imported and contains a group of functions that can be used.  An `import` statement consists of the `import` keyword, the name of the module, and optionally comma seperated more modules.

An alternative form of importing is `from random import *` which gives access to all child functions without requiring its module name before it, but can make for less readable code.

- `random` adds `random.randint()` taking 2 args - lowest and highest
- `sys` adds `sys.exit()` to end a program early.
- `pprint` adds `pprint.pprint()` (nicer way to print dictionaries - lines per item instead of all on one) and `pprint.pformat()` (returns a string of that)

---

##### Importing third party modules

Open cmd or terminal, and use pip/pip3 for Windows/MacOS and enter:
```
pip install --user modulename
```