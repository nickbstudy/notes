#### Eager Loading Related Data

To load all related data:
```
_context.Authors.Include(a => a.Books).ToList();
```
This could be filtered:
```
var old = new DateTime(2000, 1, 1)

var oldBooks = _context.Authors.Include(a => a.Books).Where(b => b.PublishDate < old)
var bAuthors = _context.Authors.Where(a => a.LastName.StartsWith("B")).Include(a => a.Books).ToList();

```

`Include()` is a method of DbSet, so must be run before the final part of the query (`Find`, `FirstOrDefault`, `ToList`, etc).

You can take this even further with `.ThenInclude(t => t.NextThing)` for loading children of the `Include` method.

Alternatively, if you have multiple children you can just use several `Include` on the parent.

You can skip a 'generation' too, e.g. `_context.Authors.Include(a => a.Books.PrintingVersions).ToList();` - this would load all authors, and all versions of each of the books, but not the books themselves.

This can cause performance issues, so be wary.  You can use `AsSplitQuery()` if you are having issues (not sure why though...).

---

#### Projecting Related Data into Queries

If you only want selected fields you can create an anonymous type through the `Select` statement. 

```
var unknownTypes = _context.Authors
    .Select( a => new 
    {
        AuthorId = a.AuthorId,
        Name = $"{a.FirstName.First}. {a.LastName}",
        Books = a.Books

    })
    .ToList();
```

This would could be sent back in a DTO (or even a struct).  The author will be untracked, as the context doesn't have its full details.  Only the books would be in the ChangeTracker.

---

#### Lazy Loading

This is disabled by default, as it is easy to use inefficiently.  It allows you to access all related elements of an entity without using `Include`.  To enable it:

- Every single navigation property on the model must be `virtual`
- Add the `Microsoft.EntityFramework.Proxies` package
- Enable it in the optionsBuilder with `optionsBuilder.UseLazyLoadingProxies()`

Be cautious of using aggregate methods when you have this enabled, as it will load every property of every entity into the ChangeTracker.  

Another common mistake is lazy loading when no context is in scope.  If the request is made after the context has been disposed an exception will be thrown.

---

#### Chaining `Include` Requests

If you have multiple related tables you want to include in your query, use the `.ThenInclude` method.  Beware this can cause high load if there is a lot of data.  An example of retrieving and working with this data:

```
var authorGraph = _context.Authors.AsNoTracking()
        .Include(a => a.Books)
        .ThenInclude(b => b.Cover)
        .ThenInclude(c => c.Artists)
        .FirstOrDefault(a => a.AuthorId == 1);

Console.WriteLine(authorGraph?.FirstName + " " + authorGraph?.LastName);
foreach (var book in authorGraph.Books)
{
    Console.WriteLine("Book:" + book.Title);
    if (book.Cover != null)
    {
        Console.WriteLine("Design Ideas: " + book.Cover.DesignIdeas);
        Console.Write("Artist(s):");
        book.Cover.Artists.ForEach(a => Console.Write(a.LastName + " "));

    }
}
```

