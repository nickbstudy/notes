Add all this to a project by importing in `app.module.ts` the `ReactiveFormsModule` (or for standalone just add that in the components import)

Each field needs to know:
- Input Value
- Validity
- Dirty
- Touched

And the form needs to know all this too. 

`FormGroup` tracks a collection of `FormControl` - This is part of the FormModel.  `FormBuilder` can be used to create large numbers of controls, and populate them all with something like this:

```
ngOnInit() {
    const contactId = this.route.snapshot.params['id'];
    if (!contactId) return
    this.contactsService.getContact(contactId).subscribe((contact) => {
      if (!contact) return;
      this.contactForm.setValue(contact);
    })
  }
```


The `ControlValueAccessor` Directive is used to achieve this, using writeValue and onChange to facilitate data flow in each direction.

Easy way to create an element for each object in an array is to have a model for the type, import it in the TS, then in the template:
```
<select formControlName="addressType">
    <option *ngFor="let addressType of addressTypes" [value]="addressType.value">
        {{ addressType.title }}
    </option>
</select>
```

`FormArray` can be added in using form builder too for dynamic adding of components