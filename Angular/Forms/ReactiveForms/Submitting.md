Include a button with `type="submit"` and in the form element add `(ngsubmit)="methodToCall()"`

Back in the component, note that `this.contactForm.value` is now a JSON object containing all the values.  This will not include any diable field values - if you want to include those use `.getRawValue()`

If you are still getting an error for not including all required fields change the service method parameter to expect a `Partial<T>` instead of just `T`