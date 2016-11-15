# Styleguides

Relevant topics. Discuss and compare different approaches to backup our choices.

## Rules

Nothing here is a dogma. We keep an option to reconsider everything as we discover more arguments.

### Rule 1

Write and style code in a language-agnostic way. Portability of knowledge and reading / writing skills is a goal.

### Rule 2

Dealing with choice in **Rule 1**, prefer syntax and semantics of better languages (Haskell, etc.).

### Rule 3

Avoid complicated grammar rules.

## Applications

### `if` vs `switch`

Prefer `if` over `switch` by default.

### `let` vs `const`

`const` implies immutability which it does not provide (so lies)

```js
const xs = []
xs.push("oops")
```

The **only** difference between `const` and `let` is 

```js
let x = 1   // ok
x = 2       // ok
const y = 1 // ok
y = 2       // error
```

The problem comes with JS imperative constructs

```js
const result = null
if (flag) {
  result = "foo" // nope
} else {
  result = "bar" // no way
}
```

So your code **will** contain `let` statemets anyway. It's noisy and inconsistent to declare variables
depending on them being overriden or not down the code. If you remove `if` which forced you to use `let` will you update
the latter to become `const`? If you have time to bother yourself with such things you probably need to reconsider
your priorities. 

Two statements for variable assignment violate **Rule 1**.<br/>
`const` violates **Rule 2**, `let` – satisfies.

Other people on the same topic:
* https://mathiasbynens.be/notes/es6-const#comment-32
* https://mathiasbynens.be/notes/es6-const#comment-41
* https://mathiasbynens.be/notes/es6-const#comment-42

### Double quotes vs Single quotes

Double quote are in-line with Haskell, Elm, Clojure conventions. Single quotes are not.<br/>
Following **Rule-2** we use double quotes as default one. Single quotes are used when it's convenient to avoid escaping.
More arguments: http://michalzalecki.com/why-i-use-doublequotes-in-javascript/

### Semicolons

There are much more rules for "where you need to put semicolon" than 
rules for "where you can't omit semicolon". Following **Rule 3** we discard semicolons
from our code. No problems so far :)

### Currying

We use `curry` (with `=>`) as a replacement for `function` word almost everywhere.<br/>
It's just a matter of reading / writing habits.

We're pretty convinced that enforced currying leads to pretty APIs while "named args" are for composability hell.<br/>
Our experience with Python (ubiquitous "named args"), Haskell (ubiquitous currying)<br/>
and other langs makes a strong evidence for that.

Once you stop writing functions with non-intuitive arguments you'll never want
to specify their "names" at call time again. And once you start typing functions with "named args" API
you'll never want to go that way.

Rule of thumb: "named args" are for kids.

### Triple equals

It's a commonplace to get advices like *always use `===`, never use `==`*. 
So you'll see `typeof foo === "function"` like if `typeof` could return something besides `String`.
It has become a dogma-like and dogmas generally make you a worse programmer.

The thing is: you always need to think of types. Even in dynamic langs like JS. Than if something can be of unpredictable type (which is a warning by itself) you use manual type coercion (`String(x) == String(y)`). This is how you would behave in almost all other languages (**Rule 1** and **Rule 2**).

Comparison rules in JS are fundamentally broken (in mathematical sense). 

```js
[] == []  // false
[] === [] // false
```

You totally can't fix that fact by "always use `===`" rule. Like previously mentioned palliatives (`const`)
this one pushes you into "safety" mindset which is a self deceit.

Besides that. There are no strict equivalents for `>`, `>=`, `<`, `<=`. 
Being simple, consistent and language agnostic is always better than trumpeting minor details
loosing the whole picture.

We recommend to use `==` where it's appropriate.

## P.S.

Check out my [Subjective Fullstack](https://github.com/ivan-kleshnin/subjective-fullstack) book.
