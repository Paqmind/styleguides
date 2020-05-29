# Paqmind styleguides

Preconfigured [.eslintrc](https://gist.github.com/ivan-kleshnin/5e71698b8179ee18cd80616a3e2305f1).

### Rules

Nothing here is a dogma. We keep an option to reconsider everything as we discover more arguments.

#### Rule 1

Write and style code in a language-agnostic way. Portability of knowledge and reading / writing skills is a goal.

#### Rule 2

Dealing with choice in **Rule 1**, prefer syntax and semantics of better languages (Haskell, etc.).

#### Rule 3

Avoid complicated grammar rules.

### Applications

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
depending on them being overriden or not somewhere down the code. If you remove `if` which forced you to use `let`, will you update
the latter to become `const`? If you have time to bother yourself with such things you probably need to reconsider
your priorities. 

`const` also can't help with function argument reassignments which are the core things that should not be reassigned.
So, as every not-working palliative solution – we propose to ditch it.

To underline: 
* multiple types of variable assignments violate **Rule 1**.<br/>
* `const` violates **Rule 2**, `let` – satisfies.

Variable reassignment are easy to manage and debug because their effect is scoped (unlike mutations).

Other people on the same topic:
* https://mathiasbynens.be/notes/es6-const#comment-32
* https://mathiasbynens.be/notes/es6-const#comment-41
* https://mathiasbynens.be/notes/es6-const#comment-42

Yehuda Katz is not a fan of const [as well](https://twitter.com/wycats/status/798710635743748096).

#### Double quotes vs Single quotes

Double quote are in-line with Haskell, Elm, Clojure conventions. Single quotes are not.<br/>
Following **Rule-2** we use double quotes as default one. Single quotes are used when it's convenient to avoid escaping.
With double quotes, it's easier to convert data from JS to JSON and vice-versa.
More arguments: http://michalzalecki.com/why-i-use-doublequotes-in-javascript/

#### Semicolons

There are much more rules for "where you need to put semicolon" than 
rules for "where you can't omit semicolon". Following **Rule 3** we discard semicolons
from our code. No problems so far :)

#### Triple equals

It's almost a mantra in JS community to *always use `===`, insead of `==`*. 
So you'll often see `typeof foo === "function"` like if `typeof` could suddenly return non-string value.
Almost noone questions this rule. Well, let's discuss it here.

Do you really think the following should  return `false`?

```
"1" == 1
```

Personally, I would say both `true` and `false` results aren't good. It's not that `"1"` and `1` aren't equal. It's that
you shouldn't compare values of different types unless you're ok with their auto-coercion. The thing is: you always need to think of types. This is how you would behave in almost all other languages (**Rule 1** and **Rule 2**). 

Then I can tell you, from my own  JS experience (spanning 5+ years) that I has exactly ZERO bugs caused by `==`. Just like with `let` used "instead of" `const`. And practice is one of the best truth criterias we have.

And besides that, there are no strict equivalents for `>`, `>=`, `<`, `<=`. Do you people complaining about that? Meh, no.

Ditto: we recommend to use `==` whenever it's appropriate (most of the time, if you don't mess types).

### P.S.

Check out my [Subjective Fullstack](https://github.com/ivan-kleshnin/subjective-fullstack) book.
