A function that given input parameters create decorators that we can apply.  This involves wrapping the decorator method in a similarly named function, then passing an argument to the decorator (or empty brackets).

```
type MessagePrefix = {
    phrase: string,
    times: number
}

// Decorator Factory
function prefix(stuff: MessagePrefix) {

    // Decorator Function - name is no longer required
    return function(target: any, context: any) {

        // Decorator Method
        const prefix = function (this: any, ...args: any[]) {
            for (let i: number = 0; i < stuff.times; i++) { // note we now need to use stuff.times since we are accessing an argument
                console.log(stuff.phrase); // as above
            }
            target.apply(this, args);
        }
        return prefix;
    }
}

class BasicThing {
    @prefix({phrase: 'blah', times: 2})
    publishLine() {
        console.log('This is a line');
    }
}

let theThing = new BasicThing();
theThing.publishLine();

// Output:  blah
            blah
            This is a line


```