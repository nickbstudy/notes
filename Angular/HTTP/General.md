### Assume all example have had `HttpClient` injected as `http`

**Setting up in standalone apps:**
In app.config.ts add `provideHttpClient` to the providers and import.

If using module apps import in `app.module.ts`

---

#### get

**Getting & parsing JSON**
By default JSON is expected from a get request.  If headers are required you can create an HttpHeaders variable (or embed directly as an object).

Parameters can be embedded using
```
this.http.get<Stuff[]>(this.apiURL, {
    params: new HttpParams().set('sort', 'desc')
})

---

Use `debouce(ms go here)` to delay requests (such as frequent keypresses)