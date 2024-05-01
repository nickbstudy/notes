 In the DbContext file you can override SaveChanges to make changes before the final result is returned.  You will still need to return an int with the number of changed items, and run the `base.SaveChanges()` method.

 ```
 public override int SaveChanges()
 {
    // perform your custom logic here
    // you can access items through ChangeTracker.Entities()
    return base.SaveChanges()
 }
 ```

 