# JavaScript generators

To bring you up to speed please read 👉 [previous post about iterables]({% post_url 2024-03-08-iterables %}).

In the spirit of learning by doing let's refactor the Range class from the previous post to use the generator function.

<details>
  <summary>
  Implementation of <b>Range</b> class
  </summary>

{% highlight js %}
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
  [Symbol.iterator]() {
    return this
  }
}

for (const i of Range(0, 10)) {
  console.log(i) // logs numbers 0 to 9
}
{% endhighlight %}
</details>

<p></p>

Here is the implementation using the generator function.

{% highlight js %}
function* range(from, to) {
  for (let i = from; i < to; i++) {
    yield i;
  }
}

for (const i of range(0, 10)) {
  console.log(i) // logs numbers 0 to 9
}
{% endhighlight %}

A couple of things to unpack:
- What's the asterisk near the *function* keyword?
- What's *yield* returning?
- What's *yield*?
- Where's *return*?

The _function*_ denotes we are creating a generator function.

{% include alert.html text="Note that there is no such thing as an arrow function generator." %}

Because generators implement the same interface as iterables, *yield* is returning:\
`{ value: (yielded value), done: boolean }`

Yield is a special keyword controlling execution. It returns a value from the generator and stops execution until the next yield keyword.

{% highlight js %}
function* g() {
  console.log('before yield1');
  yield 1;
  console.log('after yield1');
  yield 2;
  console.log('after yield2');
  return 3;
}

const generator = g();
generator.next();
// executes console.log('before yield1'); { value: 1, done: false }
generator.next();
// executes console.log('after yield1'); { value: 2, done: false }
generator.next();
// executes console.log('after yield2'); { value: 3, done: true }
//                    value returned by return ^^^^
generator.next(); // => { value: undefined, done: true }
{% endhighlight %}

Now we have another question: How is returned value handled in iterables.

{% highlight js %}
const generator2 = g();
// I will omit other console log outputs
for (const v of generator2) {
  console.log(v);
}
// 1
// 2
{% endhighlight %}

If we now try to access the returned value from `generator2` by calling `generator2.next()`
we won't get `{value: 3, done: true}`.

We will get `{value: undefined, done: true}` instead.

**Why is that?**

For of loop iterates iterable until property `done: true` is returned. The value is already consumed
by for loop and thus not available in the next `.next()` call.


## Yield as an input

There is this one thing about the yield keyword we haven't discussed yet.

Yield can bring data into the generator from outside scope.

The `.next()` function can accept one argument that will be accessible during the current
execution step.

Let's look at what's going on during generator execution.

{% highlight js %}
function* g2() {
  const a = yield 10;
  const b = yield 20;
  const c = yield 30;
  console.log({a,b,c});
}

const gen3 = g2();
console.log(gen3.next(1));
console.log(gen3.next(2));
console.log(gen3.next(3));
console.log(gen3.next(4));
{% endhighlight %}

What code is being executed?

<details open>
<summary><code>gen3.next(1)</code></summary>
<p>Argument in the first <code>next()</code> is discarded because we don't have a previous yield expression to evaluate it from.</p>

{% highlight js %}
// yield 10 is returned to the next() caller.
const a = yield 10;
{% endhighlight %}

<p>⚠️ Variable <code>a</code> is undefined at the moment.</p>
</details>

<details>
<summary><code>gen3.next(2)</code></summary>
{% highlight js %}
const a = yield; // Here first we evaluate yield as an expression that equals 2 (passed value). Now a = 2
const b = yield 20; // yield 20 is returned to the next() caller
{% endhighlight %}

<p>⚠️ Variable <code>b</code> is undefined at the moment.</p>
</details>

<details>
<summary><code>gen3.next(3)</code></summary>
{% highlight js %}
const b = yield; // yield evaluated to 3, now b = 3
const c = yield 30; // yield 30 is returned to next() caller
{% endhighlight %}

<p>⚠️ Variable <code>c</code> is undefined at the moment.</p>
</details>

<details>
<summary><code>gen3.next(4)</code></summary>
{% highlight js %}
const c = yield; // { value: undefined, done: true } is returned to the next() caller
console.log({a,b,c}); // { a: 2, b: 3, c: }
{% endhighlight %}
</details>


## Stopping the generator

Generator object expose apart from `next()` method also [`.throw()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw) and [`.return()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/return).

We can _send_ either `throw new Error(message)` or insert `return` at the current suspension point inside the generator.

Returning is practical for early stopping. Throwing an error is useful for testing purposes when the generator
can throw errors during execution and we want to simulate these errors from outside.

## How are generators useful?

Generators are handy when you would like to use streams that are consumed by smaller chunks.
They can more readable alternative to closures<sup><a href="#r1">📎1</a></sup>.

Processing data in chunks reduces memory pressure but increases overall processing time.

Below is a code sample of more advanced usage of generators.\
To learn more about what it does 👉 [check the repo](https://github.com/rybarix/mapflow).

<div style="padding: 8px; max-height: 400px; overflow: auto;" markdown="0">
		<script src="https://gist.github.com/rybarix/a624c341f90e93897d643c5b696a3c08.js"></script>
</div>

{% include nlcard.html %}

# References

- [MDN generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator/)

<sup><span id="r1">📎1</span></sup>Closure example:

{% highlight js %}
const closure = () => {
  let innerState = 0;
  return () => {
    return innerState++;
  }
}

const counter = closure();
counter(); // => 0
counter(); // => 1
{% endhighlight %}

Alternative to closure using a generator:

{% highlight js %}
function* closureAlternative() {
  let innerState = 0;
  while(true) {
    yield innerState++;
  }
}
const counter2 = closureAlternative();
counter.next().value; // => 0
counter.next().value; // => 1
{% endhighlight %}