DbContext must be able to identify a parent and child to define the relationship.

EF Core will recognize a relationship automatically if each object has a property for the other, the child will need a property for the parents ID (otherwise you can have a mapping for this).

Dependents are optional by default (though you can map it to be required).

