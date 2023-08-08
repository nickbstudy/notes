## Arrays

#### Creating
```
int[] nums = new int[3]; // fixed length
string[] colors = {"red", "blue", "yellow"};
```

#### Multi-dimensionals

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

---

## Collections

#### Lists

Strongly typed, lists can be added to/deleted as required.  Declaring an int list:
```
List<int> employeeIds = new List<int>();
employeeIds.Add(123);
employeeIds.Add(234);
int numEmployees = employeeIds.Count; // would be 2
if(employeeIds.Contains(123))
{
    Console.WriteLine("123 is found!");
}
employeeIds.Clear(); // removes all elements
```