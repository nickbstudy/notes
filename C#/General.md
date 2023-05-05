# C#

This is currently a quick throwing together of any syntax that could be useful to refer to.

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