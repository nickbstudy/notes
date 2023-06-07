- `int`
- `float`
- `str` - surround with single quotes
- `bool` - capitalise `True` and `False`
- `None` - also capitalised, replaces `null` or `undefined`

When variables are assigned new values the old type is forgotten.

Variable names can only contain letters, numbers and underscores.  They can't start with a number.

Global and Local scope function as usual, if you want to edit a global from inside a function create it first with `global`:

```
glob = "glob"
def change():
    global glob
    glob = "glob2"
change()
print(glob)  # prints "glob2"
```

