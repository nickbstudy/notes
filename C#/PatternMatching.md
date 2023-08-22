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