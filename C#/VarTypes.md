### Declaration:

- Names must start with A-z or _  
- Can contain numbers  
- Can start with @ if you want to use a keyword as an identifier


```
type varName;
type one, sameType, another;

e.g.
string firstName
```

Prefix with const to lock it in:

```
const string firstName = "Nick";
```

--- 

### bool

true or false as usual

---

### char

Represents a single character, surrounded by single quotes

---

### Whole number types

Can be signed or unsigned - signed goes pos/neg, unsigned is 0+

Generally `int` is fine, is -2b to 2b, [other options here](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)

### Floating point numbers

float < double or decimal
Use decimal for currency, avoids rounding errors with floating point types - when declaring decimals end with `M` and float with `f`:  
`decimal fiveCents = .05M;`

---

### String

Technically an array of chars (can access with `name[index]`), surrounded by "" - can escape with `\`  `\t` for tab, `\n` for new line.  Loads of built in methods.

---

### DateTime

```
DateTime today = DateTime.Now;
DateTime ymd = new DateTime(2000, 1, 1);
DateTime ymdhms = new DateTime(2000, 1, 1, 14, 30, 45);
Console.WriteLine(ymdhms.Ticks);  // Ticks are the base storage unit
```

**Parsing:** Any valid culture format works, very flexible.  Use `Parse()` or `TryParse()`:
```
DateTime d3 = DateTime.Parse("1/5/2022 14:30:45");
DateTime d4 = DateTime.Parse("Thu Jan 6, 2022");
```

**TimeSpan Type** can add or subtract values from dates and is used to calculate differences.  Negative values can be used too.
```
DateTime d1 = DateTime.Now;
DateTime d2 = new DateTime(2000, 1, 1);
TimeSpan ts = d1.Subtract(d2);  // How long since start of the century?
Console.WriteLine($"{ts.Days}d, {ts.Hours}h {ts.Minutes}m {ts.Seconds}s");
```

**Increment/decrement dates** use `.AddYears` down to `.AddMilliseconds` and it will return a new date object.  
`DateTime oneYearAway = DateTime.Now.AddYears(1).AddMilliseconds(-1234);`

---

### Converting types

```
string target = "5";
int example = int.Parse(target);
```

Every type has a .ToString() method (which is automatically invoked in Console.Write expressions)

---

### Enums

Multiple constant values can be defined using the `enum` keyword and an identifier name, either after all the other 'top level' statements in the main Program.cs file or in a separate .cs file.  The `enum` class can use `GetName()` to reveal the name at a specified index, or `IsDefined()` to return bool of its existence.  Reference specific indexes (5 shown) with `(enumName)5`

```
var daysType = typeof(Days);
string name = Enum.GetName(daysType, 1);
bool flag = Enum.IsDefined(daysType, "Mon");

Console.WriteLine($"\nFirst Name: {Days.Sat}");
Console.WriteLine($"1st index: {(int) Days.Sat}");
Console.WriteLine($"\n2nd index: {name}");
Console.WriteLine($"Contains Mon? {flag}");
Console.ReadKey();


enum Days { Sat, Sun, Mon, Tue, Wed, Thu, Fri };

// literal values will be:  Sat 0 Sun True
```

---

### Dictionary

Used to store unique key/value pairs - declared as follows:
```
Dictionary <string, string> SomeName = new Dictionary<string, string>();
```

It has `.Add` and `.Remove` methods and `.Key` and `.Value` properties.






















