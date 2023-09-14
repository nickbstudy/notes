The "Query syntax" tends to be more verbose and easily readable, whereas the "Method" syntax is more like a chain of commands.

####Simple return all:

Query syntax: 
```
list = (from prod in products select prod).ToList();
```
Method syntax:
```
list = products.Select(prod => prod).ToList();
```

---

#### Sorting
Query syntax has `orderby prod.Prop` (with optional descending after), and comma separated further columns to sort on, all between `from prod in products` and `select prod`.
Method syntax chains methods together for sorting instead:
```
list = products.OrderBy(prod => prod.Color).ThenByDescending(prod => prod.Name).ToList();
```

---

#### Filtering
Query syntax uses the `where` keyword in the middle, and works for multiple requirements the and operator is `&&` or the or is `||`

```
(from p in products 
 where p.Name.StartsWith("L") && p.Cost > 200
 select p);
```

Method syntax starts with `.Where` instead of `.Select`, and multple/or operators are used in the same way:

```
products.Where(p => p.Name.StartsWith("L") && p.Cost > 200)
```

---

#### Chunks

Separates an IEnumerable into arrays of the size specified in `.Chunk()`
```
list = products.Chunk(5).ToList();
```


