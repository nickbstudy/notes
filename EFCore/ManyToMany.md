To set up a many-to-many relationship, 
- 2 items with a <list> of each other
EF will create a table of OneTuther

- Joining
Create if require, `_context.RelatedThing.Add(otherThing);`

- Querying
Eager: `_context.Thing.Include(a => a.Others).FirstOrDefault(a => a.ThingId == 1);` will return all properties of Thing 1 and any Others with a related ID