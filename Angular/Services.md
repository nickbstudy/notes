`ng g s service-name` in the CLI to create a secvice.  This sets a global service for dependency injection.  Decorated with @Injectable({ providedIn: 'root' }) - meaning it can be used anywhere.  **Services are singleton by default.**

Generally the services should contain state and business logic, with components handling their display logic.

Fill the service class with the required properties and methods, then add it to the constructor in classes which require it and use accordingly:

**`cart.service.ts`:**
```
import { Injectable } from '@angular/core';
import { IProduct } from './catalog/product.model';

@Injectable({
  providedIn: 'root'
})
export class CartService {
  cart: IProduct[] = [];
  constructor() { }
  add(product: IProduct) {
    this.cart.push(product);
    console.log(`product ${product.name} added to cart`);
  }
}
```
And inside a consuming TS class:
```
constructor(private cartSvc: CartService) { }

addToCart(product: IProduct) {
    this.cartSvc.add(product);
}
```
It can alternatively be written as a property as follows:
```
private cartSvc: CartService = inject(CartService)
```
Although this can lead to difficulty writing unit tests.
