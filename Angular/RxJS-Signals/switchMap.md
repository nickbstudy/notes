### What is switchMap?

`switchMap` is an RxJS operator that transforms items emitted by an observable into a new observable, and then flattens the result. The key feature is that it cancels any previous inner observable when a new value comes in.
Parts of switchMap

```ts
source$.pipe(
  switchMap(value => innerObservable)
)
```

Let's analyze each part:

### 1. The input value before the arrow =>

- This is a parameter representing each value emitted from the source observable
- It can be named anything (like term, value, event, etc.)
- This parameter contains whatever was emitted by the previous observable in the chain


### 2. The function body after =>

- This is where you return a new observable
- This observable can be an HTTP request, another stream, or any observable


### 3. How it processes data

- For each value emitted by the source, it creates a new inner observable
- If the source emits a new value before the previous inner observable completes, it cancels the previous one
- Only emissions from the most recent inner observable are passed downstream

```ts
// Breaking down this example
this.searchTerms$.pipe(
  debounceTime(300),
  switchMap(term => this.httpClient.get(`/api/search?q=${term}`))
).subscribe(results => this.searchResults = results);
```
**In this example**

- `searchTerms$` is an observable that emits search strings
- `debounceTime(300)` waits 300ms between emissions to reduce API calls
- `term` is each search string emitted after debouncing
- `this.httpClient.get(...)` returns a new observable for each term
- If the user types quickly, previous HTTP requests are cancelled
- Only the results from the latest request are delivered to the subscribe function

### Common use cases
1. **HTTP requests:**
```ts
input$.pipe(
  switchMap(value => httpService.getData(value))
)
```
2. **Dependent observables**
```ts
primary$.pipe(
  switchMap(primaryValue => secondary$.pipe(
    map(secondaryValue => {
      // combine primaryValue and secondaryValue
    })
  ))
)
```

Think of `switchMap` as saying "I'm only interested in the results of the most recent observable."