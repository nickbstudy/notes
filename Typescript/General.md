#### TypeScript Types:

- `string`
- `number`
- `boolean`
- `array`
- `enum`
- `null`
- `undefined`
- `void` - absense of a type, typically for functions which don't return a value
- `any` does exist but should be avoided if possible - it defeats the purpose of TS
- `never` - rarely used for error throwing and infinte loops

```
const name: string = "Nick";
const knowsTS: boolean = true;
const favNum: number = 42;
```

Return types are inferred - any by default, unless otherwise specified like:
```
addThings(first: number, second: number): number{
    return (first + second);
}
```

Type annotation are not necessary, the compiler can infer them, but they should be used to improve readability

#### Union Types

```
let someValue: number | string | null;
someValue = 42;
someValue = 'Nick';
```

#### Arrays

`let strArray1: string[] = ['these', 'are', 'strings'];`
`let strArray2: Array<string> = ['more', 'strings', 'here'];`