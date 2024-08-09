A more efficient way to build large forms is injecting and using the `FormBuilder`.  This removes the requirement to create a new `FormControl()` for every single line, and makes it easier to define data types & nullability.

```
private fb = inject(FormBuilder);

// the nonNullable is optional to include, but it automatically makes all fields required unless otherwise specified.
contactForm = this.fb.nonNullable.group({
    id: '',
    firstName: '',
    lastName: '',
    dateOfBirth: <Date | null>null,
    phone: this.fb.nonNullable.group({
        phoneNumber: '',
        phoneType: '',
    })
});
```

This also greatly simplifies filling the form with data - instead of setting each field individially in the initializer you can just call `this.contactForm.setValue(contact)` where contact is the full object passed in.