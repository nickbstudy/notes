#### TypeScript Types:

- `string`
- `number`
- `boolean`
- `any` does exist but should be avoided - it defeats the purpose of TS

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

