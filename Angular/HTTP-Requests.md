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