slidenumbers: true
slidecount: true
footer: [@kwakles124](https://twitter.com/kwakles124)
theme: GraphQL

## GraphQL for Real People
Samuel Kwak
Web[@BigNerdRanch](https://twitter.com/bignerdranch)

^ Hello, my name is Samuel Kwak, and I'm a web developer at Big Nerd Ranch.
^ Today, we're going to do yet another Intro to GraphQL talk... except this time, we're going to approach it from the every day developer's perspective, and why adopting GraphQL doesn't have to be a scary conversation.
^ To be noted, we're not going to talk too much about what GraphQL exactly is, instead we're going to focus on how we can leverage it as a tool for rapid, iterative development.

---
[.build-lists: true]
## Why GraphQL?

* Too much data
* Too much configuring

TODO: INSERT GIF

^ I know I said it'd be different, but we really do have to cover a base or two on why GraphQL is great for the modern web industry. With each new technology, the web continues to democratize, introducing web apps not just to first-time consumers, but also to first-time developers. This spread is not without cost however.

^ I'm sure some of you have seen how big even simple web apps can be with today's client-side frameworks, or even how big JSONs can get with simple REST API calls. Even my first API, I realized at some point, I was passing back JSONs as big as 4-5 MB to a _mobile phone_! Imagine being a developer in a rising country with extremely limited data costs, and discovering that an API you were toying with was costing you a quarter in fees per call, with data you aren't even going to use.

^ Previous solutions to the above were to try and and reduce data resolution by exposing more and more endpoints, leading to endpoint hell. On top of that, each of these endpoints need documenting, validation checks, pagination, etc, the developer overhead stacks up exponentially.

---

[.build-lists: true]
## GraphQL Highlights

* Specific fetching
* Singular endpoint
* Self documenting

---

> The best way to predict the future is to invent it
-- Alan Kay

---

# And with some other body copy

> The best way to predict the future is to invent it
-- Alan Kay