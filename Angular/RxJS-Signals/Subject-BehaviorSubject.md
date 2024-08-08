These help unify updates in the system.  Observables are created in a service to reduce tight coupling, then subscribed to in components, and embedded appropriately in templates.

We need to create our own observable to set this up, using `Subject` or `BehaviorSubject`

As far as the subscriber is concerned, an observable is read-only - only the observable creator can emit items.

A **Subject** is a special type of observable that allows values to be multicast to many observers. They are multicast - able to output to many subscribers simultaneously, unlike a traditional observer which creates a new instance for each sub.

#### Hot and Cold Observables

**Cold**
- Doesn't emit until it has a subscriber
- Subscription activates the source
- Typically Unicast (each new sub gets its own set of emissions)
- `http.get` is an example of this

***Hot***
- Creation activates the source 
- Emits without subscribers
- Usually Multicast
```
productSubject = new Subject<number>();
this.productSubject.next(42);
```

`Subject` simply emits, `BehaviorSubject` caches the most recently emitted value (or the one it was created with) and provides that to any late subscribers.  Use a `Subject` if you aren't worried about missing an emission.

#### Creation syntax
```
// Define the original as private, then expose it with another statement.  Generic typing applies - number is just an example
private productSelectedSubject = new Subject<number>();
readonly productSelected$ = this.producSelectedSubject.asObservable();

// BehaviorSubject also requires an initial value to emit for early subscribers.
private anotherSubject = new BehaviorSubject<number>(0);
readonly anotherSubject$ = this.anotherSubject.asObservable();
```

#### Emitting values
`this.productSelectedSubject.next(productId);`

Or an error:
`this.productSelectedSubject.error(errorText);`

Or to complete:
`this.productSelectedSubject.complete();`



|Observer|Subject|
|--------|-------|
