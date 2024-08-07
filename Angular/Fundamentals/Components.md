`ng generate component new-components-name`
`ng generate component folder/name`
`ng g c component-name` for short.

Generated folder will contain .css .html .spec.ts and .ts files (.spec.ts is for testing).  Best naming practices are used, for a component called 'new' these are:

```
new.component.css
new.component.html
new.component.spec.ts
new.component.ts
```

It will also reference the file in `app.module.ts` with an `import` and in the `declarations:` section

Anatomy of the TS file:

```
import { Component } from '@angular/core';  // Angular imports

// Decorator section:
@Component({
  selector: 'app-home',  // used to access component from other files.  
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css'] // an array so you can have multiple CSS files
})
export class HomeComponent {
  // Actual JS class where properties & methods live for logic
}
```
**Note the selector above is `app.home` - this is an application-wide prefix that is controlled in `angular.json` under "project" -> "your-project" -> "prefix" and you should make it appropriate to your program.  Something unique to avoid collision with third party plugins too.**

`templateUrl` and `stylesUrl` can be changed to `template` and `styles` and take an argument of the HTML (backticks work for a string literal) if it is a very small component.  `styles` will still need to be a string in an array.

```
templateUrl: './home.component.html',
```
Becomes:
```
template: '<h5>This is a tiny component</h5>',
```

#### Component Lifecycle Hooks

These lifecycle hooks happen over the life of a component, and can be used to manipulate it.  The most commonly used ones are `OnInit` (to fill something with data before creation), `OnDestroy` and `OnChanges`.  Some happen once only (marked with 1), others are repeated (marked with R)

```
R OnChanges
1 OnInit
R DoCheck
1 AfterContentInit
R AfterContentChecked
1 AfterViewInit
R AfterViewChecked
1 OnDestroy
```

To add them to a component (using OnInit as an example here) add the required hook in the `import` brackets, extend `export class YourComponent` with ` implements OnInit`, and include in the class a method of `ngOnInit(): void { // your code }` (note the **`ngOnInit`** prefix in the method)

---

#### Communicating between child/parent components
**Sending data to a child:**
Parents HTML:
```
<ul class="products">
  <li class="product-item" *ngFor="let singleProduct of getFilteredProducts()">
    <bot-product-details [product]="singleProduct"></bot-product-details>
  </li>
</ul>
```
Childs TS: Add a matching property to its class definition, decorated with @Input() (will need to import Input from @angular/core):
```
export class ProductDetailsComponent {
  @Input() product!: IProduct
}
```

**Sending data to a parent:**
The childs TS class needs an `@Output() anyName = new EventEmitter()` (can have multiple if required) and a method to `emit()` (which can include data if necessary, but not shown in this example) like so:
```
export class ProductDetailsComponent {
  @Output() buy = new EventEmitter()

  buyButtonClicked(product: IProduct) {
    this.buy.emit();
  }
}
```
Then the parent HTML component declaration watches like this:
```
<li class="product-item" *ngFor="let product of getFilteredProducts()">
  <bot-product-details [product]="product" (buy)="addToCart(product)"></bot-product-details>
</li>
```