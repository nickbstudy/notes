Caching is typically handled on the repository, the controller should be agnostic to such things (it only needs the data, not to know how it was stored/received).  Timeouts should be an appropriate range, and manually flushed when data is updated.

Different types of caching:
- In memory
- Distributed cache
- Response cache
- Output cache

----

### In Memory

Bring IMemoryCache into the repository next to the dbcontext, your constructor and declarations might look like this:

```
private readonly BethanysPieShopDbContext _bethanysPieShopDbContext;
private IMemoryCache _memoryCache;
private const string AllCategoriesCacheName = "AllCategories";

public CategoryRepository(BethanysPieShopDbContext bethanysPieShopDbContext, IMemoryCache memoryCache)
{
    _bethanysPieShopDbContext = bethanysPieShopDbContext;
    _memoryCache = memoryCache;
}
```

Then the method to get all categories:

```
public async Task<IEnumerable<Category>> GetAllCategoriesAsync()
{
    List<Category> allCategories = null;

    if (!_memoryCache.TryGetValue(AllCategoriesCacheName, out allCategories))
    {
        allCategories = await _bethanysPieShopDbContext.Categories.AsNoTracking().OrderBy(c => c.CategoryId).ToListAsync();
        var cacheEntryOptions = new MemoryCacheEntryOptions().SetSlidingExpiration(TimeSpan.FromSeconds(60));

        _memoryCache.Set(AllCategoriesCacheName, allCategories, cacheEntryOptions);
    }
  
    return allCategories;
}
```

Don't forget to manually remove it in the add/update/delete methods using `_memoryCache.Remove(AllCategoriesCacheName);`