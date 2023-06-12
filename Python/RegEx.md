All examples below are expected to already have `import re` included to save space.

Regular Expressions (or regexes) are imported from the `re` module, and simplify searching for patterns.  First you must compile a pattern to search for, then assign its result to a variable with the `search()` method, and access it with `group()`.  If no matches are found it will return `None` otherwise it will contain the items found.


```
phoneNumRegex = re.compile(r'\d\d\d-\d\d\d\d\d\d')
mo = phoneNumRegex.search('My number is 021-515984.')
print(f"Phone number found: {mo.group()}")  # Phone number found: 021-515984
```

##### Grouping results
You can surround parts of your expression with `()`and they will be stored separately.  An example of the above, with the prefix separated would be:
```
phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d\d\d\d)')
mo = phoneNumRegex.search('My number is 021-515984.')
prefix, suffix = mo.groups()
print(f"Phone number found: {prefix}-{suffix}")
```
`mo.group()` or `mo.group(0)` will still return the whole result, but you can use `mo.group(1)` for the first part wrapped in brackets, `mo.group(2)` for the next etc.  Also note **`mo.groups()`** usage in the example above; it returns a tuple containing all parts of the group, and can be used with the multiple assignment trick.

##### Escaping characters

In regexes the following characters have special meanings:
```
.  ^  $  *  +  ?  {  }  [  ]  \  |  (  )
```
If you need to search for them in your text pattern you need to excape them with a backslash:
```
\.  \^  \$  \*  \+  \?  \{  \}  \[  \]  \\  \|  \(  \)
```

##### Searching for multiple terms with the pipe |
Your search string can have multiple terms separated with `|` and when the first occurence of any is found it will be returned.
```
phoneNumRegex = re.compile(r'123|456|789')
mo = phoneNumRegex.search('My number is 456789 123')
print(mo.group())  # 456
```

You can also use the pipe to search part of a pattern, for example if you wanted to find Batman, Batmobile or Batcopter:
```
batRegex = re.compile(r'Bat(man|mobile|copter)')
mo = batRegex.search('Batmobile lost a wheel')
print(mo.group())  # Batmobile
print(mo.group(1))  # mobile
```

##### Optional matching with ?

If you want a result return regardless of whether a bit of text is there, wrap it in brackets and end with `?` - e.g. to search for 'Batman' or 'Batwoman' use `re.compile('Bat(wo)?man')`