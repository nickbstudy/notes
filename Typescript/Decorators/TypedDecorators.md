#### Adding types to decorators 

Fully typed, a simple decorator looks like this:

```
function simplePrefix<This, Args extends any[], Return>(
    target: (this: This, ...args: Args) => Return,  // or Promise<Return> if async
    context: ClassMethodDecoratorContext<
        This,
        (this: This, ...args: Args) => Return   // or Promise<Return> if async
    >,
    ) {
        const addPrefix = function(this: This, ...args: Args) {
            console.log(`line before ${context.name.toString()}`);
            target.apply(this, args);
        }
        return addPrefix;
}
```

This allows you to apply restrictions to types where necessary, such as the type of arguments passed in: `simplePrefix<this, Args extends number[], Return>`