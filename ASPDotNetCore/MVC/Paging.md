A useful class to use in creating paged lists:

```
public class PaginatedList<T> : List<T>
{
    public int PageIndex { get; private set; }
    public int TotalPages { get; private set; }

    public PaginatedList(List<T> items, int count, int pageIndex, int pageSize)
    {
        PageIndex = pageIndex;

        TotalPages = (int)Math.Ceiling(count / (double)pageSize);

        AddRange(items);
    }

    public bool HasPreviousPage => PageIndex > 1;

    public bool HasNextPage => PageIndex < TotalPages;
}
```

And an example of it in action:

**Repository**
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

**Controller**
```
public async Task<IActionResult> IndexPaging(int? pageNumber)
{
    var pies = await _pieRepository.GetPiesPagedAsync(pageNumber, pageSize);

    pageNumber ??= 1;

    var count = await _pieRepository.GetAllPiesCountAsync();

    return View(new PaginatedList<Pie>(pies.ToList(), count, pageNumber.Value, pageSize));
}
```

**View**
```
@using BethanysPieShopAdmin.Utilities;

@model PaginatedList<Pie>

<h2>Pies</h2>
<hr />

@if (!Model.Any())
{
    <p>No results</p>
}
else
{
    <table class="table table-condensed table-bordered">
        <tr>

            <th>
                Id
            </th>
            <th>
                Name
            </th>
            <th>
                Price
            </th>
            <th>Actions</th>
        </tr>
        @foreach (var pie in Model)
        {

            <tr>
                <td>@pie.PieId</td>
                <td>@pie.Name</td>
                <td>@pie.Price</td>
                <td>
                    <a asp-action="Details" asp-route-id="@pie.PieId">Details</a>
                    <a asp-action="Edit" asp-route-id="@pie.PieId">Edit</a>
                    <a asp-action="Delete" asp-route-id="@pie.PieId">Delete</a>
                </td>
            </tr>
        }
    </table>


}
@{
    var prevDisabled = !Model.HasPreviousPage ? "disabled" : "";
    var nextDisabled = !Model.HasNextPage ? "disabled" : "";
}

<a asp-action="IndexPaging" asp-route-pageNumber="@(Model.PageIndex - 1)" class="btn btn-default @prevDisabled">
    Previous
</a>
<a asp-action="IndexPaging" asp-route-pageNumber="@(Model.PageIndex + 1)" class="btn btn-default @nextDisabled">
    Next
</a>


<p>
    <a class="btn btn-primary" asp-action="Add">Add new pie</a>
</p>
```
