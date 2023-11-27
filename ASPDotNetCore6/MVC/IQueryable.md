#### IQueryable

IQueryable implements IEnumerable, and can store a query to be run more efficiently.  If you intend to filter a query, storing everything in an IEnumerable first will place unnecessary load on the server.  Instead, store it as an IQueryable then filter on that.  You will need to append `.AsQueryable` to the query.  A full example of this is:
```
IQueryable<YourModel> yourModelQueryable = _dbContext.Things.Where(i => i.Category == "Whatever").AsQueryable();

yourModelQueryable = yourModelQueryable.Take(5);
```
If the above had been placed in an IEnumerable initially, it would have taken the entire table instead of just what is required when called.

Due to security concerns, and aiming to keep the controller as lean as possible, this generally happens in the repository - typically based on arguments provided to a db call (an example is in the Paging article is below).

```
public async Task<IEnumerable<Pie>> GetPiesPagedAsync(int? pageNumber, int pageSize)
{
    IQueryable<Pie> pies = from p in _bethanysPieShopDbContext.Pies
                           select p;
    
    // var would also suffice above

    pageNumber ??= 1;

    pies = pies.Skip((pageNumber.Value - 1) * pageSize).Take(pageSize);

    return await pies.AsNoTracking().ToListAsync();
}
```