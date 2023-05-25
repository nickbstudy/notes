### While

```
int i = 1;

while (i < 5)
{
    i++;
    Console.WriteLine(i);
}
```

Or a "Do While" loop will run once at least then check a condition (don't forget the semicolon after expression):

```
do
{
    thing
} while (expression);
```

`continue` will end that iteration and go check the condition, `break` will end the loop

---

### For

`break` will cancel execution, `continue` will go to the next iteration.

```
for (int i = 0; i <= 3; i++)
{
    Console.WriteLine($"i is {i}");
}
```

---

### For Each (for arrays)

Iterates over an array, reading each of its values and doing something.  Syntax is `foreach (data-type variable-name in array-name) { statements }`  for example:

```
string[] colors = { "red", "blue", "yellow" };

foreach (string color in colors)
{
    Console.WriteLine(color);  
}
```

Or for a Dictionary:

```
Dictionary <string, string> BookList = new Dictionary<string, string>();
BookList.Add("Michael Prince", "M365");
BookList.Add("Nick Brooker", "Becoming a programmer");

foreach ( KeyValuePair<string, string> book in BookList )
{
    Console.WriteLine($"Key: {book.Key} - Value: {book.Value}");
}
```