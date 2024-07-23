Currency pipes can be used to format automatically, by adding them in after an interpolated expression.
```
<div>{{ product.price | currency }}</div>
```
Given 1 as a value would show $1.00 (locale can be changed too with `currency:'GBP'` etc)


Pure pipes (built in ones) run at first render, impure (custom built) run at every render cycle, so can easily create peformance issues.  Be cautious with them.