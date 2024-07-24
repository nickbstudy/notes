Suffix with a $ to indicate a variable is an `observable` and requires subscribing.

Emits 3 callback functions: 
`next: item => ...`
`error: err => ...`
`complete: () => ...` 
Shown above as lambdas, but they could be functions defined somewhere else.

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