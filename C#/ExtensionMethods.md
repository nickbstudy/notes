Behaves like a regular method, but can be added to sealed or system object.  Useful if you don't have access to the object or its code.  

Start by declaring a static class, then declare all members as static too.  Methods must have the first argument as `(this ThingToExtend thingToExtend)` (you can have more if needed).

You can then invoke the method on the `ThingToExtend` class as if it were a member.