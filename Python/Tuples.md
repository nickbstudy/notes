Tuples are very similar to lists, except that they can't be mutated.  Define them using `()` instead of `[]` and if there is only a single value end it with a trailing comma: `myTuple('something',)`

They are useful when you have a set of values you know won't change.  They can also perform slightly faster than just a list

You can convert tuples to lists and vice versa just like other var types:

```
tupleThis = tuple([1, 2, 3])
aNewTuple = (1, 2, 3)
listified = list(aNewTuple)
print(tupleThis)  # (1, 2, 3)
print(listified)  # [1, 2, 3]
```
