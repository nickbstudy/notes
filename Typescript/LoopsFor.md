**For:**
```
for (let i=1; 1 <= 10; i++) {
    
}
```

For in will let n be the index, for of will make n the item:

let nums: number[] = [10, 20, 30, 40, 50]
for(let n in nums) {
    console.log(nums[n]);
}
// or written as a for/of loop:
for(let n of nums) {
    console.log(n);
}
```

**While:**
```
let i: number = 1;

while (i <= 10)
{

    i++;
}
```

**Switch:**
```
let fruit: string = 'apple';

switch (fruit) {
    case 'orange':
        console.log('Orange picked');
        break;
    case 'apple':
        console.log('Apple picked');
        break;
    default:
        console.log('idk');
        break;
}
```

