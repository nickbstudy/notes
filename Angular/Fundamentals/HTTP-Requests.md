#### Observables

These are a way of dealing with asynchronous data.  They can deal with streams of data.  The basic structure is
```
let myDataObs = getMyDataObservable();
myDataObs.subscribe(userData => {
    // process data
})
```

Piping data can be done between these to modify it:

```
let myDataObs = getMyDataObservable();

myDataObs.pipe(
    // modify the data
);

myDataObs.subscribe(userData => {
    // process data
})
```

---

#### Set up a Proxy Server (just API addresses?)

If you want to configure a server to be used by default in HTTP calls:
In the src folder create a file called `proxy.conf.json` and add your details.  For instance:
```
{
    "/api": {
        "target": "http://localhost:8080",
        "secure": false"
    }
}
```
This will route all calls starting with `/api` to that target server.  `secure` was set to false since HTTPS was not being used.
Then in `angular.json` find the `"serve"` key, and under `"development"` create a line:
`"proxyConfig": "src/proxy.conf.json"`

#### HTTP Requests

**HTTP Requests are best made from a service method, rather than a component itself, to improve reusability and separation of concerns**.  When you implement HTTP requests you also need to update `app.module.ts` inside `@NgModule` add to the array of `imports:` a mention of `HttpClientModule`.

Add this to the class constructor: `private http: HttpClient` and make sure you import it:
`import { HttpClient } from '@angular/commmon/http'`

An example of a `get` request service:
```
getProducts(): Observable<IProduct[]> {   // Observable needs to be imported
    return this.http.get<IProduct[]>('/api/products');   // When this is called it will need to be subscribed to to get data.
}
```
Usage of that method from a component might look like:
```
ngOnInit() {
    this.productSvc.getProducts().subscribe(returnedProducts => {
        this.products = returnedProducts;
    })
}
```

An alternative if you don't need to return a method for the consumer to subscribe to is handing it directly on the service, A POST request might be done like this:

```
add(product: IProduct) {
    this.cart.push(product)
    this.http.post('/api/cart', this.cart).subscribe(() => {   // in the subscribe(()) you can request return information, typically whatever gets updated
        console.log(`added ${product.name} to cart`);
    });
}

```