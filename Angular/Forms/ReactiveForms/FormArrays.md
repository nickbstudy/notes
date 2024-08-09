Typically used for many elements of the same class (though it is possible to use different ones).  These can be created using a class constructor:
```
contactForm = new FormGroup({
    firstName: new FormControl(),
    lastName: new FormControl()
});

phones = new FormArray([
    new FormGroup({
        phoneNum: new FormControl(),
        phoneType: new FormControl()
    })
]);
```
Or the Form Builder (using a method as a different approach):
```
contactForm = this.fb.group({
    firstName: '',
    lastName: '',
    phones: this.fb.array([this.createPhoneGroup()])
});

createPhoneGroup() {
    return this.fb.group({
      phoneNum: '',
      phoneType: ''
    })
};
```

You can push to the array using `phones.push(new FormGroup())` or `phones.push(this.fb.group(...))`

In the template wrap the group in a div with an attribute `formArrayName="phones"`