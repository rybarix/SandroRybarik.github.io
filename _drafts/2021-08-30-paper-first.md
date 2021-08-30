# Paper first

## How to slow down and design the damn thing right.

TL;DR
Invest more time into designing your work on paper or whatever tool you have in your disposal. Do it until you understand it, then write tests to your code to see if building blocks fit together and iterate. This isn't new, but it is first step to produce better code in shorter time. You will gain confidence in your code and more importantly you will be better software engineer.



To start with little context, let me tell you about my design processes before when I was younger and dumber. I used to rush to coding ASAP when I've got an idea. I did no preparation what so ever before I opened my code editor and started spitting code.

I think there wasn't single instance when I finished without hicckups or spagethi code or some problem, the code was functional, but without tests, without structure and without art behind it.

At college I enrolled classes on software modeling, but it hasn't clicked until recently, when I was reevaluating my approach to solving problems.

The key word was **DESIGN**. Design means everything when building something functional, but what does it mean? You should think about design as a process not a thing. It is continous through the building process, it needs attention in every step it is your compass when deciding what the button / api / whatever look, feel and use.

> But Sandro I'm programmer, not designer. What do you want from me to do exactly?
> I get the design part, but it seems so generic to me...

Sure, let's get into practical stuff and build something to demonstrate my point.

We will be building javascript library to for composable views, that are require zero dependencies and zero build tools.

First thing what I do is I find atleast 20 sheets of paper and start to play with the potential API. We need to keep in mind our goals. Start with composability.

Start with pure JS example. 

```html
<div>
    <div style="color: #333333">
        <div>User name here</div>
    </div>
</div>
```

```js
const div1 = document.createElement('div');
const div2 = document.createElement('div');
const div3 = document.createElement('div');
div2.style.color = "#333333";
div2.appendChild(div3);
div1.appendChild(div2);
// OR with whitespace nesting
const div1 = document.createElement('div');
    const div2 = document.createElement('div');
    div2.style.color = "#333333";
        const div3 = document.createElement('div');
    div2.appendChild(div3);
div1.appendChild(div2);
```

The js code is already hard to read, hard to change and not comparable with html example. The first sign of problem is the nesting, to compose the html elements we need a nesting mechanism.

```
div(div(div()))
```

That isn't readable either, but let's play with whitespace.

```
div(
    div(
        div()
    )
)
```

Sure, that's better. Keep this in mind, you can enforce whitespace convention to use your apis more ergonomicly.

In our example, we're missing html properties. Please take a paper now and try to design how we attach html properties onto elements. We already figured out how to pass children down to elements.

Let's play with it.

```js
// 1) api style
div({ style: "color: #333333" }, 
    div(),
    div()
)

// 2) 
div(
    div(),
    div(),
    { style: "color: #333333" }
)

// 3) also with array, but less characters
// results in better readability in this case
div({}, [
    div(),
    div()
])

```

Before we move on, we need to try build larger examples, to see if readability will be problem or not.

```js
// 1) api style
div({ 
        style: "color: #333333",
        class: "user-profile-container",
        onmouseenter:(e) => { e.target.classList.add('hovering'); },
        onmouseleave: () => { e.target.classList.remove('hovering'); } 
    }, 
    div({ 
        style: "color: #333333",
        class: "user-profile-heading",
        onmouseenter:(e) => { e.target.classList.add('hovering1'); },
        onmouseleave: () => { e.target.classList.remove('hovering1'); },
        textContent: "User Profile"
    }),
    div({ 
        style: "color: #333333",
        class: "user-profile-email",
        onmouseenter:(e) => { e.target.classList.add('hovering2'); },
        onmouseleave: () => { e.target.classList.remove('hovering2'); } 
        onclick: () => { console.log("Email clicked"); }
        textContent: "example@example.com"
    })
)

// 2) 
div( 
    div({ 
        style: "color: #333333",
        class: "user-profile-heading",
        onmouseenter:(e) => { e.target.classList.add('hovering1'); },
        onmouseleave: () => { e.target.classList.remove('hovering1'); },
        textContent: "User Profile"
    }),
    div({ 
        style: "color: #333333",
        class: "user-profile-email",
        onmouseenter:(e) => { e.target.classList.add('hovering2'); },
        onmouseleave: () => { e.target.classList.remove('hovering2'); } 
        onclick: () => { console.log("Email clicked"); }
        textContent: "example@example.com"
    }),
    { 
        style: "color: #333333",
        class: "user-profile-container",
        onmouseenter:(e) => { e.target.classList.add('hovering'); },
        onmouseleave: () => { e.target.classList.remove('hovering'); } 
    },
)

```

As we can see, the second style feels confusing, when we're dealing with more properties and elements.
So we introduce the rule, the first argument is properties following with any number of children elements.



We did it, we created our version 0 api of composable html elements. BUT, let's explore properties of this design and find _free_ features that stems from this design.


We've got html elements as a function that returns HTMLElement. In typescript terms:

```ts
interface OurHTMLElement {
    // Function that accepts HTMLElement properties 
    // (Partial because, so we can pass only some of them),
    // and children HTMLElements as second argument.
    (props: Partial<HTMLElement>, ...children: HTMLElement[]): HTMLElement
}
```

Now we have core building block and we can create _ecosystem_ around it. For example, everything that returns `HTMLElement` can be used as children in our system.

```js
const Component = ({ title }) => {
    return div({ textContent: title })
}

const view = div({ class: "container" },
    Component({ title: "Hello!" })
)
```

Another feature we can use is `.outerHTML` property that returns string representation.
This is very powerful if we want to render on server side or use it in our tests. For instance:

```js
// You can use jest, mocha,... with jsdom.
// But just for demonstration
function testComponent() {
    const view = Component({ title: "Hello" })
    const expect = `<div><div>Hello</div></div>`
    if (view.outerHTML === expect) {
        console.log(`FAILED: Component({ title: "Hello" }), expected: ${expect}, but got ${view.outerHTML}`)
    } else {
        console.log("PASSED: Component({ title: "Hello" })")
    }
}

testComponent()
```

There is another interesting property of this design which take adavantage of our powerful building block.
Let's call it something fancy let's say _transformers_.

```js
const Padding = (element) => {
    element.style.padding = "1em";
    return element
}
// OR
const Padding2 = (element) => {
    element.classList.add("app-padding")
    return element
}

// Another possibility
const OnClick = (element, callback) => {
    element.onclick = callback
    return element
}
```

Usage:

```js
const view = div({},
    Padding(div({},
        div({ textContent: "Hello" }),
        OnClick(
            div({ textContent: "click me" }),
            () => console.log("Clicked")
        )
    ))
)
```

Here we see that we can mix everything together, create our own building blocks that follow single constrain to return `HTMLElement`.

We didn't write single line of production js, all this work was on the paper sheets. We can iterate even further if we want and explore new properties or shortcomings of this design. All we need is sheet of paper, time and pencil.
