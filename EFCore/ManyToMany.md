#### Skip Navigations

This is the most common, easiest way to work with many-to-many, allowing direct references to the entities from both ends.

To set up a many-to-many relationship, each entity model needs a prop list (don't forget the empty constructor) with a `List<T>` the other item.  EF will automatically create a table combining both of their names (these examples will use 2 tables called `OneThing` and `OtherThing`)

- **Joining**
Create if required, or load item into a var, then `_context.OtherThing.Add(otherThing);`

Full example:
```
var artist = _context.Artists.FirstOrDefault(a => a.FirstName == "Pablo");
var cover = _context.Covers.Find(4);
artist.Covers.Add(cover);
_context.SaveChanges();
```  

- **Querying**
Eager: `_context.Thing.Include(a => a.OtherThing).FirstOrDefault(a => a.ThingId == 1);` will return all properties of Thing 1 and any OtherThings with a related ID 
- **Removing a join**
```
var otherWithOne = _context.OtherThing
        .Include(t => t.OneThing.Where(o => o.OneThingId == 1))
        .FirstOrDefault(t => t.OtherThingId == 1);
otherWithOne.OneThing.RemoveAt(0); // not an EF Core method, just editing the list
_context.SaveChanges();
```

Another way is to create a `var` of the object you want to remove, like this:
```
var artistToRemove = _context.Artists
        .Include(a => a.Covers)
        .FirstOrDefault(a => a.FirstName == "Pablo");
var coverToRemove = artistToRemove.Covers
        .FirstOrDefault(c => c.DesignIdeas == "A bunch of squares");
artistToRemove.Covers.Remove(coverToRemove);
_context.SaveChanges();
```

Cascade delete will remove joins if you delete an object.

---

#### Skip with Payload

This allows database-generated data in extra columns.   You create a class to represent the join containing the fields you need, then add it to the DbContext model builder (quite complicated).  Migration would update the schema, and you keep using the skip navigations as usual, just accessing the properties when needed.

An example of this in DbContext:
```
modelBuilder.Entity<Artist>()
    .HasMany(a => a.Covers)
    .WithMany(c => c.Artists)
    .UsingEntity<CoverAssignment>(
        join => join
        .HasOne<Cover>()
        .WithMany()
        .HasForeignKey(ca => ca.CoverId),
        join => join
        .HasOne<Artist>()
        .WithMany()
        .HasForeignKey(ca => ca.ArtistId));
modelBuilder.Entity<CoverAssignment>()
            .Property(ca => ca.DateCreated).HasDefaultValueSql("GetDate()");
modelBuilder.Entity<CoverAssignment>()
                .Property(ca => ca.CoverId).HasColumnName("CoversCoverId");
modelBuilder.Entity<CoverAssignment>()
                .Property(ca => ca.ArtistId).HasColumnName("ArtistsArtistId");
```

---

#### Explicit Join Class

Essentially a DB with 2 x one-to-many relationships, and its own set of properties.  Allows for much more functionality, but more difficult to navigate and maintain (each end has no direct knowledge of each other)

---

Be cautious of recursive data - especially when serializing. ==more detail to come?==


