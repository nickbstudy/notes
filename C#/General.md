# C#

Comments:
`// double slash`
```
/*
 * Multiline comment
 */
``` 

---

### Console

```
Console.WriteLine("This is a line");
Console.Write("No linebreak after this");
Console.ReadLine();  // takes input e.g. firstName = Console.ReadLine();
```
For String interpolation start with dollar sign:
`Console.WriteLine($"Your first name is {firstName});`
`{varPercent:p0}` formats as percentage with 0 decimal places, `{varMoney:c}` formats as currency, `{varInt:D6}` shows as 6 places with leading 0s, `:N` formats with commas for thousands

---

### Random

`Random rng = new Random();`
Creates a new instance of the Random object 
`rng.Next()` method with no args generates from 0 - int.Max (~2b), 1 arg is upper limit (will generate 0+, not inc arg), 2 args generates including first arg and to 1 below second arg

---

### Parsing

`newVal = int.Parse(oldVal)`
int can be double/float/etc

If you need to validate user input you can use `TryParse` which returns true if it works or false otherwise, and returns the result through the `out` keyword:

`if (int.TryParse(input, out num))`

Turning to strings = `varName.ToString()`




---
later material:

---

**Class declarations**
```
public class TodoItem
{
    public string? Title { get; set; }
    public bool IsDone { get; set; } = false;
}
```
? after string means nullable.

To create a list of new object: `private List<TodoItem> todos = new()`

`// testing commit`