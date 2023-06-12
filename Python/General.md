```
# this is a comment
""" This is a
    multiline comment """
varName = "interpolated"
print ("Hello world")
print (f"I'm an {varName} string")
print ("I'm also an %s string" % (varName))
print ('What is your name?')
myName = input()
print ("Hey " + myName + ", your name is " + str(len(myName)) + " chars long.")
print ("Rounding 3.14159 to 2 :  " + str(round(3.14159, 2)))   #3.14
```

`None` is the new null/undefined.

Line indentation is important for defining statements instead of `{ }` but if it needs to be negated `\` can be used to indicate the statement continues on the next line.

---
##### Mutable vs Immutable

Integers are immutable values that don't change (not like const but they refer to a location in memory), so when you say `varOne = varTwo` it creates a new instance.  Lists however, are mutable, since their elements can change, so if you create a list by referencing another it will simply point to the same space in memory.

```
numbers = [1, 2, 3, 4]
newNums = numbers
newNums[1] = 'hello'
print numbers  # [1, 'hello', 3, 4]
```

##### Passing references

If you pass a reference to a mutable object as an argument, then change it within the function, the original will be affected. 

```
def eggs(someParameter):
    someParameter.append('Hello')

spam = [1, 2, 3]
eggs(spam)
print(spam) # [1, 2, 3, 'Hello']
```

##### copy() and deepcopy()

If you need to create a separate instance of a list the `copy` module has some methods to simplify this.  Make sure to `import copy` then use `newListName = copy.copy(oldListName)` or if the list contains lists use `copy.deepcopy()` instead.

##### Getting arguments

`import sys` and the `sys.argv` list will contain [0] as the path to python then any arguments as further indexes.

```
#! python3
# mclip.py - A multi-clipboard program.

TEXT = {'agree': """Yes, I agree. That sounds fine to me.""",
        'busy': """Sorry, can we do this later this week or next week?""",
        'upsell': """Would you consider making this a monthly donation?"""}
        
        
import sys, pyperclip
if len(sys.argv) < 2:
    print('Usage: python mclip.py [keyphrase] - copy phrase text')
    sys.exit()

keyphrase = sys.argv[1]

print(sys.argv)

if keyphrase in TEXT:
    pyperclip.copy(TEXT[keyphrase])
    print('Text for ' + keyphrase + ' copied to clipboard.')
else:
    print('There is no text for ' + keyphrase)
```