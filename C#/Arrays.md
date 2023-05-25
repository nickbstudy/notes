### Creating
```
int[] nums = new int[3];
string[] colors = {"red", "blue", "yellow"};
```

### Multi-dimensionals

The square brackets must have a comma for each additional index, and the size must be a comma separated list:

```
int [ , ] arrName = new int [ 2, 3 ] { {1, 2, 3} , {4, 5, 6} };  // 2 arrays of 3
Console.WriteLine($"X0, Y0: {arrName[0, 0]}");  // 1
Console.WriteLine($"X1, Y2: {arrName[1, 2]}");  // 6
```

---

Getting length: `arrayName.Length;`
Can use `foreach` loop to iterate over an array
Length can not be changed after creation.