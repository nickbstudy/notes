Each field needs to know:
- Input Value
- Validity
- Dirty
- Touched

And the form needs to know all this too. 

`FormGroup` tracks a collection of `FormControl` - This is part of the FormModel

Add it to a project by importing in `app.module.ts` the `ReactiveFormsModule`

The `ControlValueAccessor` Directive is used to achieve this, using writeValue and onChange to facilitate data flow in each direction.

