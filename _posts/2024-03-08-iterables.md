# JavaScript iterables

I bet you are familiar with `for (const x of someArray) { ... }`.

The `for` construct is using iterables under the hood. Let's dive in.

## Iterables 🔄

**What is iterable?**

An iterable is object that implements `next()` method that returns object with `done` and `value` properties.
**And** implements `[Symbol.iterator]() { return this }` method.

<details style="margin-bottom: 0.5rem"><summary>🤔 what's the difference between iterable and iterator?</summary>

An iterator implements just a Iterator protocol which is next() method returning object with done and value properties.

An iterable also implements [Symbol.iterator]() { return this } method.

We can forget about Iterator protocol and focus on iterables as they're actually useful.

</details>


Here is a minimal iterable:

```js
class Range {
	constructor(from, to) {
		this.from = from
		this.to = to
		this.current = from
	}
	next() {
		if (this.current >= this.to) {
			return { done: true, value: undefined }
		}
		const n = {
			done: false, value: this.current,
		}
		this.current += 1
		return n
	}
	[Symbol.iterator]() { // What is Symbol.iteraror? check references
		return this
	}
}

for (const i of Range(0, 10)) {
	console.log(i) // logs numbers 0 to 9
}
```

Iterables are practical in cases when you want your code to be more ergonomic and support native JavaScript constructs like for of loop.

This is a stepping stone for generators on which we will look in the next post.

Check previous post about 👉 [Logical assignment operators]({% post_url 2024-03-1-logical-ops %}).

So next time you're using `for (const i of array) {}` you know that JavaScript `Array` implements
iterator protocol and it is itself an iterable.

## References

- [See all built-in APIs that accepts iterables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#built-in_apis_accepting_iterables).
- [What the heck is `Symbol.iterator`?](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator)