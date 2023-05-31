```
nums = [1, 2, 3]
print(nums[0])  # prints 1

things = [['cat', 'bat'], [1, 2, 3, 4]]
print(things[0][1])  # prints 'cat'
print(things[1][3])  # prints 4
```

Arrays are similar to most languages, you can also use negative indexes to count from last element.

You can get a slice of a list by using `num:num` for the index, the first is a start position and will capture up to (but not including) the second.  You can also leave out the index on either side of the `:` to represent `0` or the length of the list.

```
nums = [0, 1, 2, 3, 4]
capture = nums[0:3]  # == [0, 1, 2]
capture2 = nums[3:]  # == [3, 4]
```

**`len(arrayName)`** will return the length of an array

Arrays can also be concatenated just using + or * operators:

```
stuff = [1, 2, 3]
tripleStuff = stuff * 3  # [1, 2, 3, 1, 2, 3, 1, 2, 3]
stuff += ['A', 'B', 'C']  # stuff is now [1, 2, 3, 'A', 'B', 'C']
```

**`del(list[0])`** deletes the first item and moves everything else down 1.

---

##### Looping in lists

```
catNames = ['Catmandu', 'Skippy', 'Possum', 'Blink']
for name in catNames:
    print(name);

supplies = ['pens', 'staplers', 'flamethrowers', 'binders']
for i in range(len(supplies)):
    print(f'Index {str(i)} in supplies is: {supplies[i]}')
```
First method is simpler, second gives access to the index as well if needed.  Another way to loop and get index is `enumerate`
```
supplies = ['pens', 'staplers', 'flamethrowers', 'binders']
for index, item in enumerate(supplies):
    print(f"Index: {index} - item: {item}")
```

---

##### The `in` and `not in` operators

```
'howdy' in ['hello', 'hey', 'hi']  # evaluates to False
'howdy' not in ['hello', 'hey', 'hi']  # evaluates to True
```

---

##### Multiple Assignment a.k.a. tuple unpacking

```
skippy = ['cat', 'brown', 15]
animal, color, age = skippy
print (f"You unpacked a {age} y.o. {color} {animal}")  
# This prints "You unpacked a 15 y.o. brown cat"
```

Must use exactly the same number of variables as values in the list.

