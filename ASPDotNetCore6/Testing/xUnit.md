#### Strings
```
Assert.Equal(expectedString, actualString);
Assert.StartsWith(expectedString, stringToCheck);
Assert.EndsWith(expectedString, stringToCheck);

// Some can also take optional params
Assert.Equal(expectedString, actualString, ignoreCase: true);
Assert.StartsWith(expectedString, stringToCheck, StringComparison.OrdinalIgnoreCase);
````
#### Collections
```
Assert.Contains(expectedThing, collection);
// Overload method for contains
Assert.Contains(collection, item => item.Contains(thingToCheck));
Assert.DoesNotContain(expectedThing, collection);
Assert.Empty(collection);
Assert.All(collection, item => Assert.False(string.IsNullOrWhiteSpace(item)));
```

#### Numbers
`Assert.InRange(thingToCheck, lowRange, highRange);`


#### Exceptions
`Assert.Throws<T>(() => sut.Method());`

#### Types
```
Assert.IsType<T>(thing);
Assert.IsAssignableFrom<T>(thing);
Assert.Same(obj1, obj2);
Assert.NotSame(obj1, obj2);
```