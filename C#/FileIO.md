### File IO

The `System.IO.File` class library provides methods to easily write/read files on the system.  Paths must have a double backslash to escape the character.  If you are writing strings with multiple lines use `\r\n` as a line break (automatically happens between lines in arrays for `WriteAllLines`).


- `File.Exists()` takes one arg (the path) then returns a boolean if that file already exists.
- `File.WriteAllText()` requires two arguments, the path then the content.
- `File.WriteAllLines()` takes an array of strings and writes each to a line
- `File.AppendAllText()` adds all text to a file (path, content)

#### Reading

- `File.ReadAllText()` takes a path arg and reads that text to a string, including new line and tab characters.
- `File.ReadAllLines()` takes a path arg and adds lines to an array, preserving only tabs