- Many under the hood optimizations
- Easy to migrate
- TimeProvider can be mocked.
- AOT Compilation (Limited to minimal APIs)
- Pre-empting timeout requests to send a 504 using `.WithRequestTimeout(TimeSpan.FromSeconds(5))`

---

**Blazor**

- Server Side Rendering option
- 