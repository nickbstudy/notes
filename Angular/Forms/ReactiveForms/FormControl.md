Import:
`import { FormControl, ReactiveFormsModule } from '@angular/forms';`

In your component create one a `FormControl` for each field:
`firstName = new FormControl()`

Then in the corresponding template field:
`<input [formControl]="firstName">`

This creates 2-way binding, updating `this.firstName.value` whenever it is changed.  

**Providing values to FormControls**
If default values should exist then give them as parameters when newing the control.  Otherwise, use `.setValue("value goes here");` if fields need to be pre-populated (such as viewing/editing an item).

An easy way to get the ID of an item is read it from the URL query:
```
ngOnInit() {
    const contactId = this.route.snapshot.params['id'];
    if (!contactId) return; // in case it is null
    this.contactsService.getContact.subscribe((contact) => {
        if (!contact) return;
        this.firstName.setValue(contact.firstName);
        // populate the rest of the fields as above
    })
}
```