### While

```
int i = 1;

while (i < 5)
{
    i++;
    Console.WriteLine(i);
}
```

Or a "Do While" loop will run once at least then check a condition:

```
do
{
    thing
} while (expression)
```

`continue` will end that iteration and go check the condition, `break` will end the loop

---

### For

```
for (int i = 0; i <= 3; i++)
{
    Console.WriteLine($"i is {i}");
}
```