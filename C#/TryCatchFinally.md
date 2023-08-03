Exceptions are all inherited from the base Exception - handle more specific ones higher up the chain and/or have a broad one to finish.

```
try
{
    // code to try
}
catch (SpecificException spcex)
{
    Console.WriteLine(scpex.Message)
    Console.WriteLine(scpex.StackTrace)
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message)
    Console.WriteLine(ex.StackTrace)
}
finally
{
    //this will always execute regardless of above outcome
}

```