**Types:**

- Isolated - single piece of code (service/pipe/etc)
- Integration - Tests a component & its template (shallow is just the component, deep includes children)

A test should be a complete story within the `it` - move less interesting setup into `beforeEach` and keep critical setup within the `it`. Don't worry too much about DRY, it can be a little DAMP (weird analogy, no acronym?)