Built in validators:  `required` `min` `max` (checked against numeric value) `minLength` `maxLength` (length of field)`email` `pattern` (checks for regex match) `requiredTrue` (for required checkboxes).

If using `FormControl()` (not a `FormBuilder`)  these are the second value of the argument - first being its initial value (empty or otherwise).
`firstname = new FormControl('', Validators.required)`

You can pass an array as the second argument for more validators.

If using the Form Builder, the value becomes first item of an array, a single validator (or array of them) then follows:
`firstName: ''`
Becomes:
`firstName: ['', Validators.required]` or
`firstName: ['', [Validators.required, Validators.minLength(3)]]`

This doesn't show a message, but sets a boolean validity flag in `contactForm.contgrols.firstName.valid` (`.invalid` also exists for ease of use)

You can add a warning such as:
`<em *ngIf="contactForm.controls.firstName.invalid">Please enter a first name</em>`

Typically you only want this error to display if the input has been touched, so the `*ngIf` becomes:

`*ngIf="contactForm.controls.firstName.invalid && contactForm.controls.firstName.touched"`

Since this is getting quite long it is common to create a get method for the firstName field in the component, such as
```
get firstName() {
    return this.contactForm.controls.firstName;
}
```

Which reduces the above statement to just `*ngIf="firstName.invalid && firstName.touched"`

**Specific validation error messages**
Using a control with: `firstName: ['', [Validators.required, Validators.minLength(3)]]` checking on the template can be done against each individual error using `firstName.errors?.['required']` or `firstName.errors?.['minlength']` (note lowercase l in length unlike its declaration).
An example of validating for both errors in Material (with a getter set up already):
```
<mat-form-field appearance="fill">
    <mat-label>First Name</mat-label>
    <input matInput formControlName="firstName">
    <em *ngIf="firstName.errors?.['required'] && firstName.touched">Please enter a first name</em>
    <em *ngIf="firstName.errors?.['minlength'] && firstName.touched">Must be at least 3 characters</em>
</mat-form-field>
```

**Validating form groups**
Add validation as usual for each control, then add a display below it in the template such as:
```
<em *ngIf="contactForm.controls.address.invalid && contactForm.controls.address.dirty">Incomplete Address</em>
```
N.B. the `dirty` could just be `touched` - only difference is dirty means something has been typed then erased

Consider adding a border as well, e.g. some CSS `.error` for a red border, and in the div wrapping the form group add
```
<div formGroupName="address" [class.error]="contactForm.controls.address.invalid && contactForm.controls.address.dirty">
```

**Disabling submit button while invalid**
In the template on the save button: `[disabled]="contactForm.invalid"`

**Creating custom validators**
Create a folder for validators, and something like this:
```
import { AbstractControl, ValidationErrors } from "@angular/forms";

export function restrictedWords(control: AbstractControl): ValidationErrors | null {
    return control.value.includes("test") ? {restrictedWords: true} : null;
}
```

This will check for the presence of the word "test", and mark a `restrictedWords` flag true in `errors` if it is found, otherwise returning null (standard requirement for a validator that passes).

Add this to the validators/array, then an appropriate display:
`<em *ngIf="lastName.errors?.['restrictedWords']">NO TESTING!</em>`

**Passing validator arguments**
Wrap the validator in a function to take arguments, and return the function:
```
import { AbstractControl, ValidationErrors } from "@angular/forms";

export function restrictedWords(words: string[]) {
    return (control: AbstractControl): ValidationErrors | null => {
        const invalidWords = words
            .map(w => control.value.includes(w) ? w : null)
            .filter(w => w !== null);
        return invalidWords.length > 0 ? {restrictedWords: true} : null;
    }
}
```
Then pass the arguments:  `lastName: ['', restrictedWords(['foo', 'bar'])],`

Note that `restrictedWords` does not need to be a boolean, it can be any truthy value (might want to make it a string to send back restricted words used).