==Revision below first attempt.==

- switch statements, property matching syntax, granular order, all cases must be handled (could handle default with `_`)

Some pattern types:

#### Type

Lets you write a pattern that checks which runtime type the object is:
```
if (order.ShippingProvider is NZPostShippingProvider) 
{

}
```

#### Declaration

Lets you map the matched object into a local variable of that type:
```
if (order.ShippingProvider is NZPostShippingProvider provider) 
{

}
```

#### Constant

Lets you check a property, or tuple value
```
if (order is { Total: 100 }) 
{

}

if (provider is { Name: "NZ Post" }) 
{

}
```

#### Relational
Lets you use comparison operators:
```
if(order is { Total: >100 })
{

}
```

#### Logical
Lets you use:  
- **Negated not**  `if (order is not CancelledOrder) { }` - also useful to check `if (order is not null)`
- **Disjunctive or** `if (order is not (CancelledOrder or ShippedOrder)) { }`
- **Conjunctive and** `if(order is { Total: >100 and <1000 }) { }`
These can be used together too

#### Property
Lets you check if the property has a certain value:
```
if(order is { ShippingProvider: NZPost { FreightCost: 50m } }) { }
```

#### Positional
Lets you check a pattern that is based on a tuple, or the values received when deconstructing a type :
```
if (order is (>100, true)) { } // deconstructed
if ((total, read) is (>100, true)) { } // tuple
```

#### Var
Lets you map the captured value into a local variable (even if null)

```
var result = GetOrder() switch
{
    var match => ""
};

#### Discard
Often used as a default case in the switch pattern of when you don't care about the value of a captured variable, represented by `_`

#### Parenthesized
Allows you to group different patterns:
```
var result = GetOrder() switch
{
    Priority or ((total: >100, _) and not (Cancelled or Shipped)) => "",
    _ => ""
};

---

### 8/5/24 Revision:

---

#### Pattern Matching definition:

A way to write code that determines what an object is.  Examines many aspects of something to determine an outcome.  When properly utilized they make code more readable and fluent.  Examples using an order from a warehouse might be:

**Is the type a specific sub-class?**
`order is ProcessedOrder`

**Does it contain any specific values?**
`order is ProcessedOrder { IsReadyForShipment: true }`

**Does a value conform to a given range?**
`order is CancelledOrder { Total: <100 }`
(This is called a 'Relational pattern')

An alternative to `if (Something.GetType() == typeof(TypeYouExpected))`
the simpler type checking guards against null: `if (Something is ThingYouExpected)`

The right hand side of `is` defines the pattern to check (which can be simple or complex).  

#### Switch Expressions

There is a more streamlined way to write switch statements, called "Switch Expressions". The purpose of a switch expression is to use patterns to determine which value to return.

You start with where you want to put the result (could be assigning to a local variable).  On the right hand side, you first indicate which value, tuple, or object you want to produce a switch expression for (this would normally be in the switch parenthesis), e.g.

```
decimal freightCost = order.ShippingProvider switch {
    //patterns go here in the form of an expression, which has to return a value
    NZPostShippingProvider { NextDay: true } => 100m,
    NZPostShippingProvider => 50m,
    _ => 200m
};
```

All expressions must return types that are compatible with each other (or that define an implicit conversion).

Always account for all cases, or include a default case (either `_` or `var`, depending on whether you want to capture the default)

The order of cases is important too - put the most granular cases at the top.  The compiler will catch this, and highlight something like:
```
_ => 100m,
NZPost => 50m
```

You can use these to simplify methods, and directly return a result from the switch expression:

```
private decimal CalculateFreightCost(Order order) =>
    order.ShippingProvider switch
    {
        NZPost { DeliverNextDay: true } => 20m,
        NZPost => 10m,
        _ => 0m
    };
```

