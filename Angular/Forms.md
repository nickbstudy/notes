There are 2 types: Template-Driven Forms (part of the HTML template, more simple), and Reactive Forms (using JS, more testable & complex)

#### HTML Template Driven Form:

First, in `app.module.ts` add to the `imports:` array: `FormsModule`

Then create a property in the ts to hold the value/s (typically an object, with a custom type to fit the value types required).

A two-way binding on a form item (input field etc) can be created using `[(ngModel)]="dataObject.propNameHere"` so that when the form or model is updated it is reflected in the value.  **Note: this requires a `name` attribute to be present on the element (just an internal requirement)**

Add to the form element: `(ngSubmit)="yourHandlerName()"`