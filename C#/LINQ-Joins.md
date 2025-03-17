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

---

#### Joins

Query syntax is typically easier for these.  An example is:

```
public async Task<IEnumerable<SubAreaDto>> GetSubAreasForPlantAsync(string plantCode)
{
	return await (
		// Start with PlantAreas table
		from area in _context.PlantAreas
		// Filter for the specific plant code we want
		where area.PlantAreaMajorCode == plantCode
		// Join with PlantAreaSubs table
		join sub in _context.PlantAreaSubs
			// Match PlantAreaCode from both tables
			on area.PlantAreaCode equals sub.PlantAreaCode
		// Order the results
		orderby area.PlantAreaCode, sub.PlantAreaSubCode
		// Create our DTO with the fields we want
		select new SubAreaDto(
			area.PlantAreaCode,
			sub.PlantAreaSubCode,
			sub.PlantAreaSubDesc ?? ""
		)
	).ToListAsync();
}
```

Let's break down what's happening step by step:

`from area in _context.PlantAreas`

This starts our query from the PlantAreas table
We give each row the alias 'area'

`where area.PlantAreaMajorCode == plantCode`

Filters the PlantAreas table to only include rows matching our plant code
This happens before the join to reduce the amount of data we're joining

`join sub in _context.PlantAreaSubs`

Tells EF Core we want to combine data from PlantAreaSubs
Each row from this table gets the alias 'sub'

`on area.PlantAreaCode equals sub.PlantAreaCode`

This is our join condition
Only rows where these codes match will be included in the result
This replaces our foreach loop by doing the matching in the database

`orderby area.PlantAreaCode, sub.PlantAreaSubCode`

Orders the results first by plant area, then by sub area
This happens in the database rather than in memory

`select new SubAreaDto(...)`

Creates our DTO objects from the joined data
We can access fields from both tables since they're joined

Here's the same query in method syntax if you prefer that style:

```
public async Task<IEnumerable<SubAreaDto>> GetSubAreasForPlantAsync(string plantCode)
{
	return await _context.PlantAreas
		.Where(area => area.PlantAreaMajorCode == plantCode)
		.Join(
			_context.PlantAreaSubs,
			area => area.PlantAreaCode,
			sub => sub.PlantAreaCode,
			(area, sub) => new SubAreaDto(
				area.PlantAreaCode,
				sub.PlantAreaSubCode,
				sub.PlantAreaSubDesc ?? ""
			)
		)
		.OrderBy(dto => dto.PlantAreaCode)
		.ThenBy(dto => dto.SubAreaCode)
		.ToListAsync();
}
```

---

#### Another simple method syntax example, from the Isolations program:

```csharp
DeleteCheckDto check = new();

var packagesWithPoint = await _context.PackagePoints // source table
    .Where(pp => pp.PointId == pointId) // get all relevant rows
    .Join(
        _context.Packages, // table to join on
        pp => pp.PackageId, // one value to match
        p => p.PackageId, // other value to match it
        (pp, p) => p // whatever we want to return here
    )
    .ToListAsync();

check.InTemplates = packagesWithPoint.Any(p => p.Indicator == "T");

check.PackagesUsingPoint = packagesWithPoint
    .Select(p => new NameAndDescription(
        p.PackageName ?? "(No name given)",
        p.PackageDesc ?? "(No descriptiongiven)"
        ))
    .ToList();
```


Regarding the last line `(pp, p) => p` - it's defining what the join operation should return when it finds a match between `PackagePoints` and `Packages`.

In the lambda expression `(pp, p) => p`:
- `pp` represents an item from the first collection (`PackagePoints`)
- `p` represents the matching item from the second collection (`Packages`)
- The `=> p` part means "return just the package object"

The parameter names could technically be anything. They're just variables in the lambda expression. For example, these would all work the same:

```csharp
(packagePoint, package) => package
(point, pkg) => pkg 
(x, y) => y
```

What matters is the position (first parameter is from the first collection, second is from the second) and what you choose to return after the arrow `=>`.

If you wanted to return a custom object with specific properties from both collections, you could do:
```csharp
(pp, p) => new { 
    PackageId = p.PackageId, 
    PointId = pp.PointId,
    PackageName = p.PackageName 
}
```

But since you need the full package object to check the Indicator and access other properties later, returning just `p` is the simplest approach.