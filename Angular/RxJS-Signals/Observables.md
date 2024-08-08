#### Observables

Observables are a collection of events of data values emitted over time.  They are created from an event or data source
- User actions
- Application events (routing, forms)
- Response from an HTTP request
- Internal structures

Suffix variables with a $ to indicate it is an `observable` and requires subscribing.

They emit 3 callback functions: 
`next: item => ...` - `item` can be anything such as primitives, events, objects, an array, observable, or http response
`error: err => ...`
`complete: () => ...` 

They can be synchronous or asynchronous (depending on the behaviour of the source), and have finite or infinite emissions.  The above examples are lambdas but they could even be a function defined elsewhere.

---

If you subscribe you should always unsubscribe later too (typically in `ngOnDestroy`), to help prevent bugs and memory leaks.

Basic observer layout:
```
const observer = {
    next: thing => console.log(`got ${thing}`),
    error: err => console.log(`error: ${err}`);
    complete: () => console.log('done.');
}
```
typically written the other way around, subscribing directly instead of creating an observable:
```
this.sub = things$.subscribe(
    next: thing => console.log(`got ${thing}`) // and others if required
)

An example of this in action:
```
subFrom!: Subscription;

this.subFrom = from([1, 2, 3, 4]).subscribe({
    next: item => console.log(`from from: ${item}`),
    complete: () => console.log(`no nums left`)
})
```

#### Pipes

Pipes can be put between the observer and `subscribe()` to allow manipulation of data, such as filter, tap(used to log to console), take(number here), and many others - list is on RxJS documentation site