```
async function doAsyncWork() {
    let results = await GetDataFromServer();
    console.log(results);
}

console.log('Calling server...);
doAsyncWork();
console.log('Results will be displayed when ready...');
```