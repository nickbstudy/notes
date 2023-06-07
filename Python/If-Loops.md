Boolean operators are:

- `and`
- `or`
- `not`

Also use `== != < <= > >=` as usual

**Truthy and Falsey values:** `0` `0.0` and `""` will all evaluate to `False`, while any filled values will return `True`.

---

### If

```
if myGuess < randomNum:
    print("Too low")
elif myGuess > randomNum:
    print("Too high")
else:
    print("Correct!")
```

---

### While

```
spam = 0
while spam < 5:
    print('Hello, world.')
    spam += 1
```

`break` will exit the loop, `continue` will go to the start and re-evaluate condition.

---

### For loops

For loops use the `range()` function with up to 3 arguments.  If given 1 arg it will increment from 0 that many times, 2 args will start at the first and finish 1 before the last, and a third arg will define the amount to increase.

```
for i in range(3):
    print(i)
```
prints 0 1 2
```
for i in range(3, 6):
    print(i)
```
prints 3 4 5

Negative numbers can be used too.