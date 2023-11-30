> *Debugging Is the Verification of Assumptions*

If you are stuck take a break, go for a walk, come back refreshed.

- Write a unit test to prove the problem, then when it is fixed you can ensure it doesn't return.

> All exceptions should be left to bubble up to the next level in the stack - **unless** the currect execution space can restore the execution state to the premises that the current code relies on.

---

#### Client side debugging

Don't forget there are more than just `console.log` - use `.info` `.assert` (shows as error if false) `.warning` and `.error` for more clarity.

If you need to place breakpoints in your code go into the browsers Sources tab, click the chevron and select "Workspace", then Add Folder and select your dev folder (and allow the browser access).  This is more reliable than VS breakpoints, and you can use watch for variables too.

---

#### Don't just `catch` if you aren't going to do something about it

**And don't be lazy and `catch(Exception ex)`** - if you're going to catch an exception deep in the code you should be specific about it, and be ready to actually handle it.

It is generally better practice to just use a `try` (and if required `finally`) than adding in a `catch` just for the sake of it.  Exceptions should only be handled if you believe you will be able to resolve the issue, otherwise let them bubble up to be caught gracefully at the highest level where a generic message is displayed to the user (they don't need to know specifics).

Exception handling should be exceptional, try to handle the exception before it happens.

An example of error handling code:
```
var document = GetDocument();

if (document != null) {
    document.Open();

    // do stuff
    document.Close();
}
else {
    // handle the fact that there's no document
}
```
If you are able to do anything about the document failing to load the above is better, otherwise this would be more appropriate:

```
var document = GetDocument();

try {
    document.Open();

    // do other stuff
    document.Close();
}
catch(DocumentIsNullException) { }
```

---

#### Top Level Error Handling

A good TLEH does this:
 - Logs the error
 - Notifies someone that this happened
 - Securely notifies the user that the first two have happened (or will happen)
 - **Never breaks** - must be as simple and resilient as possible.

 MVC framework comes with Error.cshtml in the Shared page to use for this.  In Program.cs this segment configures it:
 ```
 if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
}
```

If an exception bubbles up to the top of a call stack, that is what will be hit.  You can comment out the `IsDevelopment` for testing purposes.

Error.cshtml (or the controller) should include these things:

- A list of ignorable errors (you may not care about 404/303 errors etc) - good razor implementation might look like
- Logging of the error
- A method to notify developers by whatever means decided


RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;

var exHandler = HttpContext.Features.Get<IExceptionHandlerPathFeature>();

if (exHandler != null) 
{
    if (ErrorIsIgnorable(HttpContext, exHandler.Error))
    {
        return;
    }

    _logger.LogError(exHandler.Error.ToString())
}