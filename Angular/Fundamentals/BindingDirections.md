Bindings can be one-way or multi-directional.  One way attribute bindings will be wrapped with [] or (), with something like [value]="propName.propValue" going from the model to the template, and conversely () pointing from the template to the model (often as a handler, pointing at methods).

Multi-directional bindings, like the form binding `[(ngModel)]="propName.propValue` will go both ways.