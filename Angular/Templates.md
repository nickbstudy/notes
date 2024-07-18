Interpolation is the process of putting expressions into the HTML that Angular will evaluate using `{{ }}`

Restrictions do apply to what you can do in a template, it won't parse complete JS, but it will evaluate expressions, or run methods declared (including passed parameters) in the component TS.

Binding to component data is a common use: Add the field (and initialize in constructor or similar) to comonent.ts file, then use curly brackets to reference it.  You can also bind attributes directly by wrapping them in square brackets (as demonstrated in `img` tag below).  This is a **one-way binding** and will not be updated in the ts if a value is changed.:
```
<ul class="products">
    <li class="product-item">
        <div class="product">
            <div class="product-details">
            <img [src]="'/assets/images/robot-parts/' + product.imageName" [alt]="product.name" />
            <div class="product-info">
                <div class="name">{{ product.name }}</div>
                <div class="description">{{ product.description }}</div>
                <div class="category">Part Type: {{ product.category }}</div>
            </div>
            </div>
            <div class="price">
            <div>${{ product.price.toFixed(2) }}</div>
            <button class="cta">Buy</button>
            </div>
        </div>
    </li>
</ul>
```
