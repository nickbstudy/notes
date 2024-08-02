Think of signals as a box which glow when a value is changed.  They must be read with `signalName()`, and updated using `set(newValue)` or `update(value => value * 2)` (use a spread for an array/object, e.g. `this.cartItems.update(items => [...items, { product, quantity: 1}])` )

Computed signals update when any of the values (must be signals) change, and are readonly.
`totalPrice = computed(() => this.selectedProduct().price * this.quantity());`

Effects are an operation that runs whenever one or more signal values change (such as logging)
`effect(() => console.log(this.selectedThing()))`
Helpful in debugging signals.

Avoid using signals for state changes, and updating other signals - this can cause unwanted domino effects.