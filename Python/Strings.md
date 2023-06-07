Escape character is `\` - use for quotation marks, `\t` for tab, `\n` for newline, `\\` for backslash

```
print(r'Starting with r will ignore \n all escape chars')
print(f'Starting with f adds {varValue} usual features')

test = 'interpolation'
print('This uses %s' % (test))  # 'This uses interpolation'

print('''Multiline strings can be
    started with triple quotations, and follow

any new line and tabs, with the compiler ignoring indentation etc...''')
```

Indexes and `in`/`not in` work with strings just like lists.

```
test = 'Hello World'
print(test[0])  # H
print(test[0:2])  # He  -start index, end index (not included)
print('Hello' in test)  # True
print('hello' in test)  # False
```

`strName.upper()` and `strName.lower()` return as that case (not changing the original)

These all return boolean values: 

- `isalpha()` Returns True if the string consists only of letters and isn’t blank

- `isalnum()` Returns True if the string consists only of letters and numbers and is not blank

- `isdecimal()` Returns True if the string consists only of numeric characters and is not blank

- `isspace()` Returns True if the string consists only of spaces, tabs, and newlines and is not blank

- `istitle()` Returns True if the string consists only of words that begin with an uppercase letter followed by only lowercase letters

- `varName.startswith('check')` Returns True if varName starts with 'check'

- `varName.endswith('check')` Returns True if varName ends with 'check'

`'hello'.isalpha()  # == True`

---

##### The join() split() and partition() methods

`join()` is called on a string value and passed a list.  It then concatenates the list with the string between items:  `', '.join(['a', 'b', 'c'])  # 'a, b, c'`

`split()` does the opposite - it is called on a string value and returns a list of strings.  If given no args it will split on whitespace (not including the spaces), or a separator arg.

```
test = "My name is Simon"
print(test.split())  # ['My', 'name', 'is', 'Simon']
print(test.split('m'))  # ['My na', 'e is Si', 'on']
```

`split` can also be used to split on `\n` 

`partition()` returns a tuple of before, the separator, and after.  If the separator string can't be found it will just return a tuple with the whole string, then two empty strings.  You can use the multiple assignment trick to assign the three returned values to variables too:

```
test = "Hello World"
first, separator, ending = test.partition(' ')
print('%s%s%s' % (first, separator, ending))  # "Hello World"
```

---

##### Justifying text

The `rjust()` and `ljust()` string methods return a padded version of the called on, with spaces inserted to justify the text.  The first arg specifies the final width, and an optional second can specify the character to use as padding.  `center()` will center text it is called on, again with first arg being width and an optional second being the character to use (otherwise spaces).

---

##### Removing whitespace

The `strip()` method will return the string with all whitespace characters (spaces, tabs, and newlines) from the beginning and end of a string.  Alternatively you can use `lstrip()` or `rstrip()` to just trim one side.  A single arg can be passed to indicate which characters to trim (order is not important):

```
spam = 'SpamSpamBaconSpamEggsSpamSpam'
print(spam.strip('ampS'))  # 'BaconSpamEggs'
```

---

##### Numeric values with ord() and chr()

Characters can be converted to their ascii number using `ord('A')` and vice versa with `chr(65)`, which can be helpful for alphabetizing

---

##### Copying and pasting strings with the Pyperclip module

The `pyperclip` is a 3rd party module which has `copy()` and `paste()` functions to send and receive text from your clipboard.