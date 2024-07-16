Decorators are functions that are applied to code, and are similar to attributes in C#.

They can be applied to:
- Classes
- Methods
- Properties
- Fields
- Getters/setters

Common uses are

- Logging
- Metrics/Tracing
- Caching
- Transactions
- Callbacks

---

#### Anatomy of a decorator

In the simplest form they are a function which is applied to code.

```
function myDecorator(target, context) {  // target is whatever the decorator is applied to, context
    const writeSomething = function (this: any, ...args: any[]) {
        console.log('snuck in there...');
        target.apply(this, args);
    }
    return writeSomething;
}

class BasicThing {

    @myDecorator
    publishLine() {
        console.log('This is a line');
    }
}

let theThing = new BasicThing();
theThing.publishLine();

---

#### Method Decorators

Method Decorators are a function that returns another function that should be executed instead of the existing class definition.  It can completely replace it, but more commonly changes a feature of it.  Once inside the inner funtion to be returned, the `this` keyword refers to the function being decorated.  A working example:

```
class Project {

    budget: number = 900;

    @withBudget(500)
    fixBugInProduction() {
        console.log('Fixing bug in production...');
    }
}

const project = new Project();
project.fixBugInProduction();
project.fixBugInProduction();

function withBudget(actionBudget: number) {
    return function <T extends {budget: number}>(target: Function, context: ClassMethodDecoratorContext<T>) {
        return function(...args: any) {
            const instance = this as T; // this refers to the base calling function - Project in this case.
            if (instance.budget > actionBudget) {
                instance.budget = instance.budget - actionBudget;
                target.apply(instance, args)
            } else {
                console.error(`Insufficient budget for ${context.name.toString()}.  Required ${actionBudget}, available ${instance.budget}`);
            }
            return target
        }
    }
}
```

This will result in one successful call, then one error logged as budget then < 500

---

#### Field Decorators

```
type Task = {
    name: string,
    level: 'low' | 'medium' | 'complicated'
}

class Manager {

    @withComplicatedTask
    tasks: Task[] = []
}

const manager = new Manager();
console.log(manager);

function withComplicatedTask<T, V extends Task[]>(target: undefined, context: ClassFieldDecoratorContext<T, V>) {
    return function (args: V) {
        args.push({
            name: "added task",
            level: "complicated"
        })
        return args;
    }
}
```

In defining the decorator for a field, target has to be undefined, and the context<T, V> represent Type, and Value.  The declaration has to have `V extends Task[]` so args knows what types it can work with.

You can write a decorator factory like the following `withTask` to create then, and then run like a function in the code:

```
type Task = {
    name: string,
    level: 'low' | 'medium' | 'complicated'
}

class Manager {

    @withTask({
        name: 'added task',
        level: 'low'
    })
    tasks: Task[] = []

    @withComplicatedTask()
    addedTasks: Task[] = []
}

const manager = new Manager();
console.log(manager);

function withTask(task: Task) {
    return function <T, V extends Task[]>(target: undefined, context: ClassFieldDecoratorContext<T, V>) {
        return function (args: V) {
            args.push(task)
            return args;
        }
    }
}

function withComplicatedTask() {
    return withTask({
        name: 'added task',
        level: 'complicated'
    })
}
```
---

#### Accessor Decorators