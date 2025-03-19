If you have a large set of transactions to make, and want to ensure they all complete before comitting to the database you should use a transaction:

```csharp
using var transaction = await _context.Database.BeginTransactionAsync();

try 
{
	// Lots of complex logic would go here, even multiple SaveChange calls if you need to generate new IDs etc

	await _context.SaveChangesAsync();
	await transaction.CommitAsync();

	return Result.Success(); // or whatever you're returning
}
catch (Exception ex)
{
	await transaction.RollbackAsync();
	return Result.Failure($"Failed to complete transaction: {ex.Message}");
}
```

This guarantees that if anything goes wrong no changes will be made to the database, only a complete call with no issues will be committed.