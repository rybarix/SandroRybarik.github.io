# JavaScript ES6 and beyond

It has been 9 years since JavaScript ES6 was introduced.

In ES6 release we've got important features as: lexical scoping ie. `let` and `const`, classes, arrow functions, template literals, destructuring, maps, sets and default parameters.

I clearly rembember how this release changed writing of JavaScript.

But what about subsequent releases? What have we got since then?

## 2016 ES7 has brought:
- Exponentiation operator
  - `const square = 4**4`
- `Array.prototype.includes()`
  - `[1,2,3].includes(3) // true`

## 2017 ES8 has brought:
- `async` `await` support
- `Object.entries`
  - `Object.entries({ a: 1, b: 2 }) // [['a', 1], ['b', 2]]`
- `Object.values`
  - `Object.values({ a: 1, b: 'hello' }) // [1, 'hello']`
- `Object.getOwnPropertyDescriptors`
- String padding functions: `padStart`, `padEnd`

## 2018 ES9 has brought:
- Async iterator support<sup><a href="#r1">📎1</a></sup>
- Async generator<sup><a href="#r2">📎2</a></sup>
- `Promise.prototype.finally()`<sup><a href="#r3">📎2</a></sup>

## 2019 ES10 has brought:
- `Array.prototype.flat()`
  - `[1,2,[3,4]].flat() // [1,2,3,4]`
- `Array.prototype.flatMap()`
  - `[1,2].flatMap((x) => [x,x*2]) // [1,2,2,4]`
- `Object.fromEntries()`
  - `Object.fromEntries([['a', 1], ['b', 2]]) // {a:1,b:2}`
- String.prototype new functions `trimStart()` `trimEnd()` `trimLeft()` `trimRight()`

## 2020 ES11 has brought:
- BigInt support<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)</sup>
- `Promise.allSettled()`<sup><a href="#r4">📎4</a></sup>
- Nullish coalescing operator `??`
- Optional chaining `?.`
- globalThis<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)</sup>
- `export * as ns from 'module'` syntax for modules
- `import.meta`<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta)</sup>

## 2021 ES12 has brought:
- `String.prototype.replaceAll()`
  - `'aaabbb'.replaceAll('a','c') // cccbbb`
- `Promise.any()`<sup><a href="#r5">📎5</a></sup>
- Logical assignment operators `??=`, `&&=`, `||=`
- `WeakRef` referencing object but allowing it to be garbage collected
- `_` separator allowed in number literals
  - `1_000_000 // 1000000`

## 2022 ES13 has brought:
- top-level `await`
- public, private, static class fields, static initialization blocks<sup><a href="#r6">📎6</a></sup>
- `#x in obj` syntax, to test for presence of private fields on objects
- `\d` new RegExp flag<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/hasIndices)</sup>
- `TypedArray`<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)</sup>
- `cause` property on `Error` objects useful when re-throwing error to access original error.<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/cause)</sup>
- `at()` for Strings, Arrays and TypedArrays
  - `'abcd'.at(-1) // d`, `'abcd'.at(2) // c`
- `Object.hasOwn()`<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)</sup> a better `Object.prototype.hasOwnProperty()`

## 2023 ES14 has brought:
- New methods on `Array.prototype`
  - `toSorted()` returns new copy of sorted array
  - `toReversed()` returns new copy of reversed array
  - `with()`<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/with)</sup>
  - `findLast()`
  - `findLastIndex()`
  - `toSpliced()`
- `#!` shebang support


## 2024 ES15 has brought:
- `\v` new RegExp flag<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/unicodeSets)</sup>
- `Promise.withResolvers()` convinient promise construction<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers)</sup>
- `Object.groupBy`, `Map.groupBy` now easier to group items based on property/value<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy)</sup>
- `Atomics.waitAsync()` useful when working with shared memory buffers<sup>[🌎↗](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics/waitAsync)</sup>
- `String.prototype.isWellFormed()` and `String.prototype.toWellFormed()` checks for well-formed Unicode

What is your favourite feature from above list?


If you enjoyed this article you can join my newsletter to get dose of weekly craftsmanship.

[Get dose of Craftsmanship here](https://news.rybarix.com)

---

## References

- [tc39/ecma262](https://tc39.es/ecma262/multipage/#sec-intro)
- [webreference](https://webreference.com/javascript/basics/versions)
- [mdn](https://developer.mozilla.org/en-US/)

<sup id="r1">📎1</sup> Async iterator support ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncIterator))
```js
const asyncIterator = (async function* () {
  yield 1;
  yield 2;
  yield 3;
})();
(async () => {
  for await (const value of asyncIterator) {
    console.log(value);
  }
})();
// Logs: 1, 2, 3
```

<sup id="r2">📎2</sup> Async generator support ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncGenerator))
```js
async function* createAsyncGenerator() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
  yield await Promise.resolve(3);
}
const asyncGen = createAsyncGenerator();
asyncGen.next().then((res) => console.log(res.value)); // 1
asyncGen.next().then((res) => console.log(res.value)); // 2
asyncGen.next().then((res) => console.log(res.value)); // 3
```

<sup id="r3">📎3</sup> Promise <code>finally()</code>([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally))

Schedules a function to be called when the promise is settled (either fulfilled or rejected).

```js
fetch('https://api.example.com/json-data')
  .then(response => response.json())
  .then(json => console.log(json))
  .catch(error => console.log(error))
  .finally(() => console.log('finally() called!'));
```

<sup id="r4">📎4</sup> <code>Promise.allSettled()</code> ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled))

This returned promise fulfills when all of the input's promises settle (including when an empty iterable is passed), with an array of objects that describe the outcome of each promise.

```js
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 100, 'foo'),
);
const promises = [promise1, promise2];

Promise.allSettled(promises).then((results) =>
  results.forEach((result) => console.log(result.status)),
);
// Expected output:
// "fulfilled"
// "rejected"
```

<sup id="r5">📎5</sup> <code>Promise.any()</code> ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/any))
```js
const promise1 = Promise.reject(0);
const promise2 = new Promise((resolve) => setTimeout(resolve, 100, 'quick'));
const promise3 = new Promise((resolve) => setTimeout(resolve, 500, 'slow'));
const promises = [promise1, promise2, promise3];
Promise.any(promises).then((value) => console.log(value));
// Expected output: "quick"
```

<sup id="r6">📎6</sup> [public](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields), [private](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties), [static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static) class fields, [static initialization blocks](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Static_initialization_blocks)

```js
class ExampleClass {
  instanceField;
  instanceFieldWithInitializer = "instance field";
  static staticField;
  static staticFieldWithInitializer = "static field";
  #privateField;
  #privateFieldWithInitializer = 42;

  static {
    this.staticField = 'Static initialization block'
    // Why? We can use here try...catch block.
    // More than one of these blocks can be declared.
  }

  publicMethod() {}
  static staticMethod() {}
  #privateMethod() {}
  static #staticPrivateMethod() {} // ExampleClass.#staticPrivateMethod()
                                   // allowed to be called only within class
}
// The name of a static property (field or method) cannot be prototype.
// The name of a class field (static or instance) cannot be constructor.
```