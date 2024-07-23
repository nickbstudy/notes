Template:

Most of the work is done in the HTML template.  Still uses FormControl and FormGroup behind the scenes.  Has a 2-way binding on the `[(ngModel)]` attribute.

Reactive:

FormControl is created as a property in the class with `ctlName = new FormControl('')`, then a 1 way binding to the component is created with `[formControl]="ctlName"` being added to the element.  Data makes its way back to the class through event binding.