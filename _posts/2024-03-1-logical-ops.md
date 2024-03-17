# Logical and nullish assignment operators in JavaScript

Before we dive in, let's first define truthy, falsy, and nullish values.

**Falsy** are: `null`, `undefined`, `false`, `NaN`, `0`, `-0`, `0n` a BigInt zero, `""`, `document.all`.

**Truthy** are: any other value than falsy is considered truthy.

**Nullish** are: `undefined`, `null`.

Why is `document.all` falsy? [Follow the rabbit hole here](https://stackoverflow.com/questions/10350142/why-is-document-all-falsy){:target="_blank" rel="nofollow}.

Logical assignment operators are `&&=`, `||=` and the nullish coalescing assignment is `??=`.

Here is an overview of what they do:

* `x ||= y` is equivalent to `x || (x = y)`. Assigns y to x when **x** is **falsy**.
* `x &&= y` is equivalent to `x && (x = y)`. Assigns y to x when **x** is **truthy**.
* `x ??= y` is equivalent to `x ?? (x = y)`. Assigns y to x when **x** is **nullish**.


## When to use what?

You don't see these operators in the wild that much because they can be confusing at first.
But there are cases when they're useful! 💪
I tried to come up with rules that may be helpful when deciding whether to use this or not.

### ||=

Use it when updating values that can be falsy.
For `undefined` and `null` types there is no difference between `||=` and `??=`.

```js
function createPost(title, body) {
  title ||= 'Untitled'
  body ||= 'Empty body'
  // ...
}
// Which is the same as:
function createPost(title, body) {
  if (title === '') {
    title = 'Untitled'
  }
  if (body === '') {
    body = 'Empty body'
  }
  // ...
}
```

### &&=
Use it when updating truthy value.
The snippet below should give you a rough idea about proper usage.
You can update an already set value easily and skip the update when the value hasn't been initialized yet.

```js
const config = {
  user: undefined, // this can be also { user: string, password: string, admin: boolean }
  host: 'https://example.com',
}

// Will be executed only if a user is present
// Set user to admin
config.user &&= {
  ...config.user,
  admin: true
}

fetch('https://example.com/api/resource', {
  headers: { 'content-type': 'application/json' },
  body: JSON.stringify(config)
})
```

### ??=

Use it when setting a new value on nullish or uninitialized variable.
It is also useful for saving expensive computation.
See snippet below:

```js
class Fetcher {
  users // undefined at first

  // Fetching is an expensive operation, we can do it only once
  // and then serve users from this.users variable.
  async getUsers() {
    this.users ??= await fetch('http://example.com/api/users')
			.then(r => r.json())
    return this.users
  }
}
```

```js
/** @param {% raw %}{{ location: string|undefined }}{% endraw %} jobPost */
function createJobPost(jobPost) {
	jobPost.location ??= 'remote'
	return jobPost
}

createJobPost({}) // => { location: 'remote }
```

## Conclusion

With these operators we can remove some if statements guarding whether value is defined or not.

I recommend using `&&=` and `??=` for most of the time.
Nullish values are more common for default values. In that case there is `??=` for you.

When you want to update a value only when it is defined the `&&=` operator will serve you well.


Check previous post about 👉 [JavaScript ES6 and beyond]({% post_url 2024-02-21-es6-and-beyond %}).

## References

- [Logical OR assignment (\|\|=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment){:target="_blank" rel="nofollow}
- [Logical AND assignment (&&=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND_assignment){:target="_blank" rel="nofollow}
- [Nullish coalescing assignment (??=)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_assignment){:target="_blank" rel="nofollow}
