An event binding on an HTML element is the event wrapped in brackets = "the expression to be run".  This would change the property `filter` to `'Heads'` when you click the element:
```
<a class="button" (click)="filter='Heads'">Heads</a>
```
These are one-way, going only from the component to the template to the model.