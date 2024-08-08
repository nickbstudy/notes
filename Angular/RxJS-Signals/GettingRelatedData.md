#### Higher Order Mapping Operators

The 3 types are: `concatMap`, `mergeMap`, and `switchMap`.  These are used inside observables when an observable is required (referred to as 'inner observables').  They transform an emitted item, automatically sub/unsub, and unwrap (flatten) the result - effectively turning an `Observable<T>` into a `T`

**`concatMap`**
`concatMap(i => of(i))`
This will wait for each inner observable to complete before processing the next one, outputting in sequence.  Use this when order is essential.  Technically a **transformation operator** - when an item is emitted, it's queued.
- Item is mapped to an inner observable as specified by the provided function
- Subscribes to the inner observable
- Waits
- Inner observables are concatenated to the output observable
- When the inner observable completes, processes the next item

**`mergeMap`**
`mergeMap(i => of(i))`
This is another **transformation operator** which runs in parallel - all values are returned as soon as they are received, in no particular order.
- Item is mapped to an inner observable as specified by the provided function
- Subscribes to the inner observable
- Inner observable emissions are **merged** to the output observable in the order they finish

**`switchMap`**
`switchMap(i => of(i))`
Transforms each emitted item to a new inner observable as defined by a function, unsubscribing and switching to the new inner observable each time one is provided - only one at a time is running.  Useful for when a user can change their mind on loading.
- Item is mapped to an inner observable as specified by the provided function
- Switches to the inner observable
  - Unsubscribes from any prior inner observable
  - Subscribes to the new inner observable

Example:
```
range(1, 5)
    .pipe(
        switchMap(i => of(i)
            .pipe(
            delay(500)
            )
        )
    )
    .subscribe(x => console.log(x))
// If there were no delay output would be 1 2 3 4 5, but with it the output is only 5.
```