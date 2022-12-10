## All settled method

```js
function wrapIt(promise) {
  // Promise allSettled will return an array of objects, we grab the first
  // That object will only every have one of these properties:
  // ✅ value - the resolved data
  // OR
  // ✅ reason - the rejected error

  return Promise.allSettled([promise]).then(function ([{ value, reason }]) {
    return [value, reason];
  });
}
```

Now we get a "tuple" - an array with two items, the first being the resolved data, the second being the rejected

We can destructure it into two variables, named whatever we want

```js
const [data, err] = await wrapIt(getCheese()); // ? [['cheddar', 'gouda'], undefined]]
const [cheese, cheeseError] = await wrapIt(getCheese(true)); // ? [undefined, 'Cheese sucks' ]
```

Some people prefer returning a "result" type

```js
function wrapItObject(promise) {
  return Promise.allSettled([promise]).then(function ([{ value, reason }]) {
    return { data: value, error: reason };
  });
}
```

Then get a result object

```js
const result = await wrapItObject(getCheese()); // {data: ['cheddar', 'gouda'] error: undefined}
console.log(result); // { data: [ 'cheddar', 'gouda'] error: undefined}
if(result.error) {  handle error}
```

Or destructure it and rename it right away in case you have multiple other "data" and "error" namings.

```js
const { data: goodData, error: crappyError } = await wrapObject(getCheese());
console.log(goodData, crappyError); // ['cheddar', 'gouda'] undefined
```

Further reading

- [Await to JS](_https://www.npmjs.com/package/await-to-js)
- [Github - Doublet](https://github.com/mats852/doublet)
- [Github - Go Go Try](https://github.com/thelinuxlich/go-go-try)
