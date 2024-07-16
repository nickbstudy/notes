#### Generic functions:

```
function LogAndReturn<T>(thing: T): T {
    console.log(thing);
    return thing;
}

let someString: string = LogAndReturn<string>('log this');

let newMag: Magazine = { title: 'Web Dev Monthly' };
let someMag: Magazine = LogAndReturn<Magazine>(newmag);
```

Not a particularly useful function but it demonstrates writing a function using a generic type.

```
function Purge<T>(inventory: Array<T>): Array<T> {
	return inventory.splice(3, inventory.length);
}

let inventory: Array<Movie> = GetAllMovies(); // fills an array with sample movies
let purgedMovies: Array<Movie> = Purge<Movie>(inventory);
```
#### Generic Interfaces

```
interface Inventory<T> {
    getNewestItem: () => T;
    addItem: (newItem: T) => void;
    getAllItems: () => Array<T>;
}

let bookInventory: Inventory<Book>

// or a class:

class Favorites<T> {
    private _items: Array<T> = new Array<T>;

    add(item: T): void {
        this._items.push(item);
    }

    getFirst(): T {
        return this._items[0];
    }
}
```
#### Generic Constraints
```
interface CatalogItem {
    catalogNumber: number;
}

class Catalog<T extends CatalogItem> implements Inventory<T> {
    // interface methods go here.
}
```

