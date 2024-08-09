**Radio buttons**
Should all have the same attribute `formControlName="fieldNameHere"` to toggle between.

**Select lists**
```
<select fromControlName="addressType">
    <option value="home">Home</option>
    // etc
</select>
```

Or to generate programmatically:
```
<select formControlName="addressType">
    <option *ngFor="let addressType of addressTypes" [value]="addressType.value">
        {{addressType.title}}
    </option>
</select>
    
</select>
```

**Checkboxes**
`<input type="checkbox" formControlName="nameOfField" /> Field Name`

**Numeric**
Just add `type="number"`

**Range**
`type="range" min="1" max="10"` 

**Textarea works just like an input**

**Date**
Use `type="date"` and if value won't populate it probably wants to have a yyyy-MM-dd format.  Achieve this by including this attribute: `[value]="contactForm.controls.dateOfBirth.value | date:'yyyy-MM-dd'"`

When submitting this will be passed as a string, not a Date object.  Date types are not natively supported in angular (or JSON), and it can be easier to work with them as strings.  Use the .ToISOString() method to help with this. [Video here](https://app.pluralsight.com/ilx/video-courses/739e63f1-c84d-48eb-bc9a-bdd1428bc8e9/6b6f831e-dd01-4be1-9e3b-e28f6be1eba3/bd0e9a38-aeb9-4c85-84bc-e697fa8f49dd)

