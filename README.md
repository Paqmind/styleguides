# Styleguides

Our JS styleguides explained. Not a dogma – it may change gradually.

## Rules

#### Rule 1

Write and style code in a language-agnostic way. Portability of knowledge and reading / writing skills is a goal.

#### Rule 2

Dealing with choice in **Rule 1**, prefer syntax and semantics of better languages (Haskell, etc.).

#### Rule 3

Avoid complicated grammar rules.

## Applications

#### `const` vs `let`

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

Besides that. Two statements for variable assignment violate **Rule 1**.<br/>
`const` violates **Rule 2**, `let` – satisfies.

#### Double quote vs Single quote

Double quote is in-line with Haskell, Elm, Clojure conventions. Single quote is not.<br/>
Following **Rule-2** we use double quote as default one. Single quote is used where it's convenient `"double-quoted-string"`.

#### Semicolons

There are much more rules for "where you need to put semicolon" than 
rules for "where you can't omit semicolon". Following **Rule 3** we discard semicolons
from our code. No problems so far :)

#### Curry

We use `curry` (with `=>`) as a replacement for `function` word almost everywhere.<br/>
It's just a matter of reading / writing habits.

We're pretty convinced that enforced currying leads to pretty APIs while "named args" are for composability hell.<br/>
Our experience with Python (ubiquitous "named args"), Haskell (ubiquitous currying)<br/>
and other langs makes a strong evidence for that.

Once you stop writing functions with non-intuitive arguments you'll never want
to specify their "names" at call time again. And once you start typing functions with "named args" API
you'll never want to go that way.

Rule of thumb: "named args" are for kids.

