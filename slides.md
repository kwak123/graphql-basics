slidenumbers: true
slidecount: true
footer: [@kwakles124](https://twitter.com/kwakles124)
theme: GraphQL
build-lists: true

## GraphQL for Real People
Samuel Kwak
Web[@BigNerdRanch](https://twitter.com/bignerdranch)

^ Hello, my name is Samuel Kwak, and I'm a web developer at Big Nerd Ranch.

^ Today, we're going to do yet another Intro to GraphQL talk... except this time, we're going to approach it from the every day developer's perspective, and why adopting GraphQL doesn't have to be a scary conversation.

^ To be noted, we're not going to talk too much about what GraphQL exactly is, instead we're going to focus on learning and applying the basic concepts of GraphQL.

---

## Why GraphQL?

* Too much data
* Too much configuring

TODO: INSERT GIF

^ I know I said it'd be different, but we really do have to cover a base or two on why GraphQL is great for the modern web industry. With each new technology, the web continues to democratize, introducing web apps not just to first-time consumers, but also to first-time developers. This spread is not without cost however.

^ I'm sure some of you have seen how big even simple web apps can be with today's client-side frameworks, or even how big JSONs can get with simple REST API calls. Even my first API, I realized at some point, I was passing back JSONs as big as 4-5 MB to a _mobile phone_! Imagine being a developer in a rising country with extremely limited data costs, and discovering that an API you were toying with was costing you a quarter in fees per call, with data you aren't even going to use.

^ Previous solutions to the above were to try and and reduce data resolution by exposing more and more endpoints, leading to endpoint hell. On top of that, each of these endpoints need documenting, validation checks, pagination, etc, the developer overhead stacks up exponentially.

---
## GraphQL Highlights

* Specific fetching
* Singular, semi-self-documenting endpoint
* Built-in validation and response handling

^ Which brings us to some of the best things that GraphQL offers developers. We'll break out of the serious mold now, and just start looking at what we get to work with, and that starts with knowing where GraphQL leaves us.

^ First thing's first, what does GraphQL stand for? Query Language, but what exactly does that mean? It's confusing, you imagine it's like SQL, and it kinda is, but really, GraphQL is just a way of asking for things explicitly. Now knowing what you want to ask for is a complicated thing. Let's just reflect on that concept. There's two big concerns with getting data in the REST spec, first is having to create new endpoints for any new piece of data, second is all the the validation that has to go in to resolving only what we want if we want that data to be configurable. Thankfully, GraphQL handles both of these big concerns.

^ First, we only have to use one end point now. GraphQL servers will receive and respond along one endpoint. This GraphQL endpoint doesn't need to response to all the various request types, the 2 that you really need to work with are GET and POST requests. Don't worry too much about the self-documenting aspect now, we'll get to that in the live coding bit.

^ Second, we get built-in validation and response handling. Specifically, all we need to teach GraphQL how to do is how to fetch data, GraphQL will take care of parsing responses, making database or other API requests, and responding to the incoming request.

^ Personally, I've found it's easier to understand GraphQL by applying it to a simile. The REST spec is sort of like the Dewey decimal system, a great, rigid structure for quickly locating books on a certain topic. Want to add a new topic, you'd need a new number, etc. GraphQL is more like the librarian, you approach GraphQL with your questions or requests, they go and fulfill it and come back to you.

^ Questions?

---

## GraphQL's Components

* Queries -> What's the count for fiction/nonfiction?
* Mutations -> Let's order more Harry Potter books.
* Fragments -> Get the author/genre for these books.
* Interfaces -> Books have title, author, and genre.
* Unions -> These things are all books.

~ Subscriptions -> Tell me when new books arrive.

^ There's 5 big basic parts of GraphQL: Queries, Mutations, Fragments, Interfaces, and Unions. Sticking with our librarian theme, we can compare our requests vs how we might ask a librarin

^ Queries are the bread and butter of a GraphQL server. They're GET requests, pretty much just a read operation.

^ Mutations are like POST requests, they're requests to update the server in any way.

^ Fragments are a way of asking the same question for many things. There are a few ways to do fragments, such as inline fragments vs declared fragments.

^ Interfaces are straight from most OOP languages, anything that implements an interface must meet that interface's contract

^ Unions are similar to interfaces, but instead of necessarily declaring a contract, you're more like associating them as synonyms.

^ Subscriptions are a little different. They aren't necessarily a part of the GraphQL spec, but most GraphQL implementations will take advantage of a subscription based model. We won't worry too much about this topic, just know that it's out there.

---

[.build-lists: false]

## Language Concepts

```ruby
type Book {
  id: ID!
  title: String
  author: [String]
}
```

^ Here's our first real part of GraphQL. At a glance, this looks a bit like a protobuf or a struct, or maybe even a schema and you'd be right. This is called Schema Definition Language, and we're using it to define a type definition. It's like the legalese of GraphQL, you define your contracts that the API will follow using this language. This little snippet contains a few important pieces of GraphQL.

---

[.build-lists: false]
[.code-highlight: 2]

## Language Concepts

```ruby
type Book {
  id: ID!
  title: String
  author: [String]
}
```

* Nullability

^ First is this idea of nullability. This is exactly the concept of null that you probably have, that in some object, that piece of data is missing, or more precisely, does not exist. The exclamation point lets GraphQL know that contractually, this field cannot be blank, and if it is blank, there's a big problem.

---

[.build-lists: false]
[.code-highlight: 1, 3]

## Language Concepts

```ruby
type Book {
  id: ID!
  title: String
  author: [String]
}
```

* Nullability
* Types/Scalars

^ Next is the concept of scalars and types. Types are data structures that contain data inside them, and scalars are the actual data itself, the piece that can't be described any smaller than it actually is. A book can have all these attributes, but when we look at the title, there's really only one title. Types are like objects, Scalars are like primitives.

---

[.build-lists: false]
[.code-highlight: 4]

## Language Concepts

```ruby
type Book {
  id: ID!
  title: String
  author: [String]
}
```

* Nullability
* Types/Scalars
* Lists

^ Last is the concept of lists. We mentioned that scalars are the most primitive unit in GraphQL, but lists are just above that. Anything can be wrapped in a list, all you're doing is informing the contract that there is a variable number of this item.

---

## Combining Concepts

Nullability, Types/Scalars, Lists

```ruby
type Author {
  name: String!
}

type Book {
  authors: [Author!]!
  related: [Book]
}
```

^ You can combine these three aspects and create just about anything. For instance, the Book type can have lists of Author types to query for, and even refer to itself in its type definition. Nullability can be applied at many levels, including within lists or even the list itself.

^ Trivia, what _can_ the authors key return?

---

## Queries

```ruby
type Query {
  getBooks: [Book]
  getAuthors: [Author]
}
```

* Query is a default GraphQL Type
* Obeys the same rules as other type defs

^ Finally! Here's our first bread and butter piece of GraphQL. Can anyone quickly try and point out anything interesting here?

^ The first interesting point is that Query is simply another GraphQL type def, except it has the added responsiblity of being a default (and required) type def.

^ Because a Query is just a type definition, it still obeys the SDL, getBooks is just some attribute of the Query type that will return you a list of Books. Here's where we really see SDL is a contract language only. Looking at the getBooks gives you that gut feeling that it somehow is responsible for getting books, but really it's just a data attribute that's part of the type Query, that if referenced, will return you a list of books.

---

## Queries

```ruby
type Query {
  getBooks: [Book]
  getAuthors: [Author]
  getBookByTitle(title: String!): Book
  getBooksByAuthor(author: Author!): [Book]
}
```

* Can query by variable
* Can also query by existing types

^ Variable Querying, where nullability can still apply. You can search for things by variable, or even by other types. For something like JavaScript, duck typing comes into play here, but there are ways to decide how an object given to GraphQL is resolved.


---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Mutations

```ruby
type Mutation {
  deleteBook(id: ID!): Book
  addBook(book: Book): Book
  addAuthor(author: Author): Author
}
```

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Fragments

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Interfaces

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Unions

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Resources

[How To GraphQL](https://www.howtographql.com)
[GitHub for Slides/Server](https://github.com/kwak123/graphql-basics)
