Each component CSS file is encapsulated to its HTML file only - no parent or child components will use them.
"Class binding" can be used similar to attributes; square brackents of class.class-name then something to evaluate a boolean:
`<div [class.strikethrough]="product.discount > 0">`
Will only be apply the strikethrough class if discount > 0

Another option that works well if you have multiple evaluations to make is the `[ngClass]` binding.  This accepts an object, which will check each of its values for truth and apply accordingly.  This can either be inline or a function in the TS file.

Inline:
`<div [ngClass]="{strikethrough: product.discount > 0}" >`

Function:
**HTML:**
`<div [ngClass]="getDiscountedProducts(product)" >`

**TS**
```
getDiscountedProducts(product: IProduct) {
    if (product.discount > 0) 
        return 'strikethrough'; // this could contain many classes, just add them all in the string
    else
        return '';
}
```

Alternatively, the TS can return an array of strings to represent the classes to add.  This would be useful for building up from complex sets of information.

#### `[style.attribute]`

Applying styles can be done using `[style.attribute-you-want-to-add]` like so:
`[style.color]="'red'"`
Note 2 sets of quotation marks are needed, as red is a string within the parameters.  It can also be evaluated:
`[style.color]="product.discount > 0 ? 'red' : ''" `
And `ngStyle` functions for this as it does for class above - except that it can't receive a string or array, it must be an object of key/value pairs.

**HTML**
`[ngStyle]="makeDiscountedRed(product)"`

**TS**
```
makeDiscountedRed(product: IProduct) {
    if (product.discount > 0) return {'color': 'red'};
    else return {};
}
```