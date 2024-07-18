For use in HTML to extend it.  You can build custom ones.  Having a * prefix means they modify HTML content (structural directives).

ngClass
*ngIf
*ngFor
*ngSwitch

#### *ngFor

Put this at the end of the element you want repeated `*ngFor="let thing of things"` and all instances will be iterations of the array:
```
<li class="product-item" *ngFor="let product of products">
    <div class="product">
        <div class="product-details">
            <img [src]="getImageUrl(product)" [alt]="product.name" />
            <div class="product-info">
                <div class="name">{{ product.name }}</div>
                <!-- etc etc -->
```

#### *ngIf

Include in an element and give an expression that evaluates to true or false:

```
<div class="price">
    <div *ngIf="product.discount === 0">
        ${{ product.price.toFixed(2) }}
    </div>
    <div class="discount" *ngIf="product.discount !== 0">
        ${{ (product.price * (1 - product.discount)).toFixed(2) }}
    </div>
</div>
```

N.B. an alternative to the above would be `[hidden]="product.discount === 0"`