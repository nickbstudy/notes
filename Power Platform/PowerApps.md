Variables:

`Set(varName, "Data here")`

Collections:

`Collect` will add a record to a collection, creating it if it doesn't already exist:

```
Collect(ShoppingCard,
{Name: "Item 1", Price: 5},
{Name: "Item 2", Price: 18})
```

`Remove(name, item)` will remove a record from a collection
`Clear(name)` will delete all records from a collection

`ClearCollect()` will clear all records then start adding new data

---

`SaveData(collectionName, "LocalGivenName")` and `LoadData()` are used for offline capability.  Saves a collection.  If(Connection.Connected) will return connection status
`ClearData("LocalGivenName")` afterwards to clean up too.

`Search('source', text, column)`