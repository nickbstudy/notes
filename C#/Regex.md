#### Anchors

Metacharacters that specify where a match must occur within a string.

- `^` - start of the string
- `$` - end of the string

Used together they can require matching of an entire pattern to succeed.  
When multiline mode is on the match must occur at the beginning/end (or before the \n at the end) of each line.

----

#### Character Groups

Defines a set of characters to match

- `[aeiou]` - match any of the chars listed
- `[a-z]` - match any lower case char in the range listed
- `[^AEIOU]` - the `^` defines this as a negative group, will fail on any of the listed chars
- `[a-zA-Z]` - match on any of its 2 groups - essentially any english char

Other character classes:

- `.` - any char (wildcard)
- `\w` - any word char (letters, digits, punctuation)
- `\W` - any non word char
- `\d` - any digit char
- `\D` - any non digit char
- `\s` - any whitespace (inc. space, tabs, newlines)
- `\S` - any non-whitespace

----

#### Quantifiers

Used to specify how many instances of a char, group, or char class must occur within the input.  They can be greedy (matching as much as possible) or lazy (matching as little as possible).  Greedy by default, they can be made lazy by following them with a `?`

- `*` - zero or more times
- `+` - one or more times
- `?` - zero or one times
- `{3}` - exactly 3 times
- `{3,}` - at least 3 times
- `{3,6}` - between 3 and 6 times
- `{,10}` - between 0 and 10 times

----

#### Subexpressions

Defined within a grouping construct, and used to extract substrings for separate processing or grouping subexpressions to apply a quantifier.  They may be capturing (for later use) or non-capturing (just for quantification).  They can be named by including a name in `<>` at the start, e.g. `(?<name>)` and the name can be reused to keep items within a group.

Subexpressions are defined within `(subex goes here)` or to make non-capturing `(?:subex goes here)
`

---

#### Examples

```
var pattern = "(?:Az){3}";
var match = Regex.Match("AzAzAz", pattern);  // True
match = Regex.Match("AzAz", pattern);  // False - only 2 repetitions
match = Regex.Match("AzWhateverAzAzAzAz", pattern);  // True, pattern is there 3+ times

---

If you are storing a regex pattern in a string and want to still have the formatting, add a comment line above it reading `//language=regex`