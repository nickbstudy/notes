### File IO

The `System.IO.File` class library provides methods to easily write/read files on the system.  Paths must have a double backslash to escape the character, or you can use the @ sign to ignore escape sequences making it just, `@"C:\dev\FolderName"`  .  If you are writing strings with multiple lines use `\r\n` as a line break (automatically happens between lines in arrays for `WriteAllLines`).


- `File.Exists()` takes one arg (the path) then returns a boolean if that file already exists.
- `File.WriteAllText()` requires two arguments, the path then the content.
- `File.WriteAllLines()` takes an array of strings and writes each to a line
- `File.AppendAllText()` adds all text to a file (path, content)

#### Reading

- `File.ReadAllText()` takes a path arg and reads that text to a string, including new line and tab characters.
- `File.ReadAllLines()` takes a path arg and adds lines to an array, preserving only tabs

---

### Streaming Lines

The above options can be limited, the `System.IO.StreamWriter` class provides object methods that can be used to read/write more efficiently by dot-suffixing.  An instance must first be created using the `new` keyword, and since it is disposable it should be inside a `using` construct (inside a try/catch to check for errors too) to ensure it is removed from memory after completion.  It can also accept a keyword of `true` to append to existing text.

```
string path = "C:\\Users\\nickb\\Desktop\\nick.txt";
string[] poem = new string[]
{
    "\tFirst Line",
    "\tSecond Line",
    "\tThird Line"
};

string lorem = "\n\tLorem...";

using (StreamWriter sw = new StreamWriter(path))
    { foreach (string line in poem) { sw.WriteLine(line); } }
using (StreamWriter sw = new StreamWriter(path, true))
    { sw.WriteLine(lorem); }
```

- `sw.Write()` appends text
- `sw.WriteLine()` appends a line

**StreamReader** acts as the opposite, using:

- `sr.Read()` reads one char at a time
- `sr.ReadAllLines()` fills a string array with all lines
- `sr.ReadAllText()` fills a string with whole file

---

More examples:

```
internal static void SaveEmployees(List<Employee> employees)
        {
            CheckForExistingEmployeeFile();
            string path = $"{directory}{fileName}";
            StringBuilder sb = new StringBuilder();
            foreach (Employee employee in employees)
            {
                sb.Append($"firstName:{employee.FirstName};");
                sb.Append($"lastName:{employee.LastName};");
                sb.Append($"wage:{employee.Wage};");
                sb.Append($"type:{GetEmployeeType(employee)};");
                sb.Append(Environment.NewLine);
            }
            File.WriteAllText(path, sb.ToString());
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Saved successfully!");
            Console.ResetColor();
        }

        internal static void LoadEmployees(List<Employee> employees)
        {
            string path = $"{directory}{fileName}";
            if (File.Exists(path))
            {
                employees.Clear();

                string[] employeesAsString = File.ReadAllLines(path);
                for (int i = 0; i < employeesAsString.Length; i++)
                {
                    string[] employeeSplits = employeesAsString[i].Split(';');
                    string firstName = employeeSplits[0].Substring(employeeSplits[0].IndexOf(":") + 1);
                    string lastName = employeeSplits[1].Substring(employeeSplits[1].IndexOf(":") + 1);
                    string wage = employeeSplits[2].Substring(employeeSplits[2].IndexOf(":") + 1);
                    string type = employeeSplits[3].Substring(employeeSplits[3].IndexOf(":") + 1);
                    

                    Employee employee = null;

                    if (type == "2")
                    {
                        employee = new Manager(firstName, lastName, double.Parse(wage));
                    }
                    else
                    {
                        employee = new Employee(firstName, lastName, double.Parse(wage));
                    }

                    employees.Add(employee);
                }

                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine($"Loaded {employees.Count} employees!");
                Console.ResetColor();

            }
        }
```