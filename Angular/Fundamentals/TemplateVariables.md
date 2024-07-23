Used heavily in forms (especially template based), their use is discouraged but sometimes required.  Add a tag to your elemends and tie it to ngModel like so:
`<input name="email" #email="ngModel">`
This gives access to things like:
```
email.valid // shows based on the required tag
email.invalid
.dirty
.pristine
.touched
.untouched
```

You can also validate a form by tagging it and using `formsTagHere.form.invalid` or `valid`