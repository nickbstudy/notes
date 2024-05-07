Blazor is component based, with all pages living in the \Pages\ folder.  You can include code on your page after the HTML with `@code { // code goes here }` - called a 'mixed approach' - but it is recommended to have it in a separate 'code behind' page using partial and have the same name.  If you have a page called `PieOverview.razor` you can create this code behind as `PieOverview.razor.cs` and declare it as `public partial class PieOverview` etc.

Dependency injection is handled differently too; instead of a constructor you just have an attribute then the interface to inject:
```
[Inject]
public IPieRepository? PieRepository { get; set; }
// then access like so:
var allPies = PieRepository.GetAll();
```

Requirements in Program.cs:

```
builder.Services.AddServerSideBlazor();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host"); //could change depending on address you want if only partially using
```
And in /Shared/_Layout.cshtml
```
<base href="~/" />
<script src="_framework/blazor.server.js"></script>
```

The file itself will have an address similar to razor, using `@page "/pagenamehere"`

If you navigate to `/pagenamehere` then that component will take up the whole screen, but if it has been built as a component you can include it in another page with `<pagenamehere />`