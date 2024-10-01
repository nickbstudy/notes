####Setting up global usings/suppressions

In the root project folder create `GlobalUsings.cs`

Populate that with rows, e.g.
```
global using dotnetAPI.DatabaseContext
global using dotnetAPI.Models;
global using dotnetAPI.Repositories;
global using Microsoft.EntityFrameworkCore;
```

A `GlobalSuppressions.cs` file can also be used to suppress common warnings (can right-click a warning and add it to this):

```
// This file is used by Code Analysis to maintain SuppressMessage
// attributes that are applied to this project.
// Project-level suppressions either have no target or are given
// a specific target and scoped to a namespace, type, member, etc.

using System.Diagnostics.CodeAnalysis;

[assembly: SuppressMessage("Interoperability", "CA1416:Validate platform compatibility", Justification = "Known to be running on Windows", Scope = "member", Target = "~M:PlantIsolations.Services.AdAuthenticator.GetUser(System.String)~System.DirectoryServices.AccountManagement.UserPrincipal")]
```