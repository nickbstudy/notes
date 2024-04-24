### `public class YourContext : DbContext`

The DbContext represents a session with the database.  It lies dormant until its first call to the database.  You can have multiple designated to operate in different ways (such as some for no-tracking for view-only access).  The `DbContext.ChangeTracker` manages a collection of 'EntityEntry' objects, which keep track of the state info for each entity:  CurrentValues, OriginalValues, State enum, Entity, and more.  In advanced cases you can interact with these.

An "Entity" is an In-Memory object that the DbContext is aware of.  It must have a `key` property (usually the Id field)

Tracking states are:
- Unchanged (default)
- Added
- Modified
- Deleted

When you call `SaveChanges()` on a DbContext it looks at the enums of each entity to decide whether it needs updating, and creates an appropriate SQL command to reflect any changes.  It returns the number of affected and/or created rows, which is why something like `bool updatedCorrectly = _context.SaveChanges > 0;` is an effective check to return proof of an expected update.  It then updates state to reflect the new state of things.

You can call `_context.DetectChanges()` to update the ChangeTracker at any time, though it is rare you would need this, as it happens automatically when you call `.SaveChanges()`

