Create and wrap a form group around all the `FormControl` items.
```
contactForm = new FormGroup({
    firstName: new FormControl(),  // Different syntax from before - swapped = for : and added ,
    lastName: new FormControl(),
    //etc...
})
```
Then in the template, update the `<form>` tag to `<form [formGroup]="contactForm">`
Any form controls should now be bound with `<input formControlName="firstName">`

When accessing individual controls, instead of referencing them directly, they now become:
`this.contactForm.controls.firstName` etc...