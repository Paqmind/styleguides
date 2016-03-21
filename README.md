# Styleguides

Relevant topics. Discuss and compare different approaches to backup our choices.

## Rules

Nothing here is a dogma. We keep an option to reconsider everything as we discover more arguments.

#### Rule 1

Write and style code in a language-agnostic way. Portability of knowledge and reading / writing skills is a goal.

#### Rule 2

Dealing with choice in **Rule 1**, prefer syntax and semantics of better languages (Haskell, etc.).

#### Rule 3

Avoid complicated grammar rules.

## Applications

#### `if` vs `switch`

Prefer `if` over `switch` by default.

#### `let` vs `const`

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

#### Double quote vs Single quote

Double quote is in-line with Haskell, Elm, Clojure conventions. Single quote is not.<br/>
Following **Rule-2** we use double quote as default one. Single quote is used where it's convenient `"double-quoted-string"`.

#### Semicolons

There are much more rules for "where you need to put semicolon" than 
rules for "where you can't omit semicolon". Following **Rule 3** we discard semicolons
from our code. No problems so far :)

#### Currying

We use `curry` (with `=>`) as a replacement for `function` word almost everywhere.<br/>
It's just a matter of reading / writing habits.

We're pretty convinced that enforced currying leads to pretty APIs while "named args" are for composability hell.<br/>
Our experience with Python (ubiquitous "named args"), Haskell (ubiquitous currying)<br/>
and other langs makes a strong evidence for that.

Once you stop writing functions with non-intuitive arguments you'll never want
to specify their "names" at call time again. And once you start typing functions with "named args" API
you'll never want to go that way.

Rule of thumb: "named args" are for kids.

#### Triple equals

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

#### Immutability

At least 7 approaches are possible here.

##### 1. Defensive copying

```js
let ys = xs.slice(0, xs.length)
ys.splice(...)
```

#### Benefits

Not found

#### Drawbacks

* exta code 
* clutter 
* error-prone

##### 2. Manual freezing

```js
Object.freeze({name: "Jack"})
```

#### Benefits

* keeps native API 

#### Drawbacks

* exta code 
* clutter 
* error-prone

##### 3. Auto freezing

[seamless-immutable](https://github.com/rtfeldman/seamless-immutable)

#### Benefits

* keeps native API 

#### Drawbacks

* exta code (for little gain)
* clutter 

##### 4. Persistent datastructures

A builtin in functional languages (Haskell, Clojure...).

[ImmutableJS](https://github.com/facebook/immutable-js)

[Mori](https://github.com/swannodette/mori)

#### Benefits

* memory usage for big datasets (games)

#### Drawbacks

* specific API (unnecessary hard to combine with other libs) 
* performance
* negatively affects bundle size (such libs are relatively heavy)

##### 5. Immutability as a side-effect 

Some libraries can apply `Object.freeze` in addition to their main purpose stuff (type validation, etc.). 

[Tcomb](https://github.com/gcanti/tcomb)

#### Benefits

* keeps native API 
* immutability as a bonus

#### Drawbacks

* specific API (unnecessary hard to combine with other libs) 
* performance
* negatively affects bundle size (such libs are relatively heavy)

##### 6. Manual "just never mutate"

```js
xs.concat([4])
// instead of
xs.push(4)
```

#### Benefits

* keeps native API 
* no additional deps / conditions

#### Drawbacks

* exta code 
* clutter 
* error-prone

##### 7. Auto "just never mutate"

[Ramda](http://ramdajs.com/)
 
#### Benefits

* keeps native API 
* immutability as a bonus

#### Drawbacks

* negatively affects bundle size (effect will significantly reduce with upcoming [tree shaking](www.2ality.com/2015/12/webpack-tree-shaking.html))

Our favorites are **5** and **7**. Use **4** when you have to deal with heavy memory load.
Never use **1**.
