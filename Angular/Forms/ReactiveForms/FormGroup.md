Create and wrap a form group around all the `FormControl` items.
```
contactForm = new FormGroup({
    firstName: new FormControl(),  // Different syntax from before - swapped = for : and added ,
    lastName: new FormControl(),
    //etc...
})
```
Then in the template, update the `<form>` tag to `<form [formGroup]="contactForm">` 
(and probably `(ngsubmit)="methodToCall()"`)

Any form controls should now be bound with `<input formControlName="firstName">`

When accessing individual controls, instead of referencing them directly, they now become:
`this.contactForm.controls.firstName` etc...

You can also add fields without a display (such as IDs) by adding a FormControl in the group, and setting it on the initializer.

**Nesting form groups**
If you have an interface that includes references to other interfaces you can use nesting to create an appropriate form group.  This also simplifies validation.

To implement this just create a new `FormGroup({})` filled with the required `FormControl()` - when submitting the easiest way to get all data is just `this.formName.GetRawValue()` (this will include all disabled forms)

Add an attribute for each div/section of the template `formGroupName="nameHere"`