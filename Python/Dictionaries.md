A dictionary is a mutable collection of many values of any type, but uses key value pairs instead of indexes.  It is typed with `{ }` and trying to access a non-existent key will produce a `KeyError` event.
```
stuff = {123: "numbers", "color": "red"}
print(stuff[123] + " " + stuff["color"])  # numbers red
```

To check if a value exists just use `if thing in dictName:`

To add a value: `dictName["keyName"] = "value"`

Dictionaries can be nested inside other dictionaries or lists.

---

##### The keys() values() and items() methods

These three dictionary methods return list-like values of a dictionaries keys, values, or both.  They aren't true lists, and won't work with append, but do work with `for` loops.  `keys()` and `values()` appear similar to lists, and `items()` is a list of tuples
```
stuff = {123: "numbers", "color": "red"}
print(stuff.keys())   # dict_keys([123, 'color'])
print(stuff.values())   # dict_values(['numbers', 'red'])
print(stuff.items())  # dict_items([(123, 'numbers'), ('color', 'red')])
```

If you need a true list from these values, pass it to the `list()` function.
```
stuff = {123: "numbers", "color": "red"}
keyList = list(stuff.keys())
print(keyList)  # [123, 'color']
```

You can also use multiple assignment with a for loop to separate the keys and values:
```
stuff = {123: "numbers", "color": "red"}
for k, v in stuff.items():
    print(f"Key: {k}  Value: {v}")
```

---

##### Defaulting

It can be tedious to check whether something exists before accessing it every time, which is where the `get()` method can come in handy.  It takes two args: the key of the value to use, and a default if it can't be found.

`setDefault()` can be used to set a value only if a key doesn't exist.  It takes two args: the value to search for, and something to set as its key if not found.  If it already exists it returns the keys value.

```
spam = {'name': 'Pooka', 'age': 5}
if 'color' not in spam:
    spam['color'] = 'black'
```