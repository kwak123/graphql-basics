slidenumbers: true
slidecount: true
footer: [@kwakles124](https://twitter.com/kwakles124)
theme: GraphQL
build-lists: true

## GraphQL for Real People
Samuel Kwak
Web[@BigNerdRanch](https://twitter.com/bignerdranch)

^ Hello, my name is Samuel Kwak, and I'm a web developer at Big Nerd Ranch.

^ Today, we're going to do yet another Intro to GraphQL talk... except this time, we're going to approach it from the every day developer's perspective, and why adopting GraphQL doesn't have to be a scary conversation. We're not going to talk too much about the big picture of what GraphQL exactly is, instead we're going to focus on learning and applying the basic concepts of GraphQL.

^ Before we start, due to time constraints, we have a couple options we can take. There are some slides describing how to use GraphQL, but there is also an option to go through it with live coding. If we do live coding, we can do it interspersed with the slides, or we can go through the slides, then do some live code together.

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

^ Second, we get built-in validation and response handling. Specifically, all we need to teach GraphQL how to do is the data should look, GraphQL will take care of parsing responses, validating, and responding to the incoming request.

^ Personally, I've found it's easier to understand GraphQL by applying it to a simile. The REST spec is sort of like the Dewey decimal system, a great, rigid structure for quickly locating books on a certain topic. Want to add a new topic, you'd need a new number, etc. GraphQL is more like the librarian, you approach GraphQL with your requests, and they make sure it's answered reasonably.

^ Questions?

---

## GraphQL's Components

* Queries -> What's the count for fiction/nonfiction?
* Mutations -> Let's order more Harry Potter books.
* Fragments -> Get the author/genre for these books.
* Interfaces -> Books have title, author, and genre.
* Unions -> These things are all books.
* _Subscriptions -> Tell me when new books arrive._

^ There's 5 big basic parts of GraphQL: Queries, Mutations, Fragments, Interfaces, and Unions. Sticking with our librarian theme, we can compare our requests vs how we might ask a librarian

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
  authors: [String]
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
  authors: [String]
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
  authors: [String]
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
  authors: [String]
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

[.code-highlight: 4]

## Queries

```ruby
type Query {
  getBooks: [Book]
  getAuthors: [Author]
  getBookByTitle(title: String!): Book
}
```

* Can query by variable

^ You can query by variables, but nullability will still apply. For something like JavaScript, duck typing may comes into play here, but there are ways to decide how an object given to GraphQL is resolved.

---

[.code-highlight: 1-3, 9]

## Queries

```ruby
input AuthorInput {
  name: String!
}

type Query {
  getBooks: [Book]
  getAuthors: [Author]
  getBookByTitle(title: String!): Book
  getBooksByAuthor(authorInput: AuthorInput!): [Book]
}
```

* Can query by variable
* Can also query by input

^ Searching using types requires you to actually use inputs; types are not the same, though very similar. The big difference is that inputs can only have scalars, other inputs, and lists of either of those as their fields. They cannot have types as a field (not super sure why)

---

## Consuming Queries

```ruby
type Author {
  name: String!
}

type Book {
  title: String!
  authors: [Author]
}

type Query {
  getBookByTitle(title: String!): Book
}

# On Client
query {
  getBookByTitle(title: "Something") {
    title
  }
}
```

^ Now we have our first glance at what GraphQL looks like on the client side... and it is dead simple. You simply call the desired query, then list off scalar fields that are part of the returned type that you wish to get data for.

---

## Consuming Queries

```ruby
type Author {
  name: String!
}

type Query {
  getBookByTitle(title: String!): Book
}

# On Client
query {
  getBookByTitle(title: "Something") {
    title
    authors {
      name
    }
  }
}
```

^ What if one the fields is a type, and not a scalar? It's as simple as creating a nested query. Note that authors was a list, but we don't need to denote that, it'll simply fetch this name field for every author.

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Mutations

```ruby
input BookInput {
  title: String!
  authors: [String!]!
}

type Mutation {
  deleteBook(id: ID!): Book
  addAuthor(name: String!): Author
  addBook(book: BookInput!): Book
}
```

^ Now, we're at the second big part of any API worth its salt, the ability to modify data. Can anyone tell me what Mutation is? (conventional, non-required type-def)

^ So what is happening here? Just like the query, we are simply declaring that this Mutation type has some data attributes that will eventually return you some data. Personally, this is where the power of convention really kicks in on GraphQL. The unfortunate nature of contractual obligation is that the fulfiller doesn't have to do what you think it will, as long as it satisfies this contract. REST at least has its various methods to try and enforce good practice, but with GraphQL, it'll be pretty heavily reliant on developer patterns.

^ Again, the same rule of searching by input applies

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Fragments

```ruby
fragment BookParts on Book {
  title
}

query {
  getBooks {
    ...BookParts
  }
}
```

^ We all hate WET code, it's painful having to refactor a drippy, soggy codebase. The same feelings apply to querying, so thankfully GraphQL gives us some great querying options. There are multiple ways to define fragments, but the easiest way is like so, simply declaring the fragment and what fields it asks for. Then we can use it in our queries.

---

[.code-highlight: 1-3]

## Fragments

```ruby
fragment BookParts on Book {
  title
}

query {
  getBooks {
    ...BookParts
  }
}
```

^ Firstly, Fragments are declared on types. In this example, we created a new Fragment called BookParts that can be given to any query, and this fragment applies to the Book type. and if that query returns you a Book in some way, it'll use this field as

---

[.code-highlight: 1-3, 6-8]

## Fragments

```ruby
fragment BookParts on Book {
  title
}

query {
  getBooks {
    ...BookParts
  }
}
```

^ If a query returns you a Book in some way, it'll include any field in the fragment as part of that query.

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Interfaces

```ruby
interface Book {
  title: String!
  authors: [String!]!
}

type Fiction implements Book {
  title: String!
  authors: [String!]!
  genre: String!
}

type NonFiction implements Book {
  title: String!
  authors: [String!]!
  era: String!
}
```

^ Interfaces are sort of like contracts... within the contract language. They function in much the same way as the do in many strongly-typed languages, anything that implements an interface must expose all fields as defined by that interface. However, this gives us our first real option to creating generic queries!

---

## Interfaces

```ruby
interface Book {
  id: Int!
  title: String!
  authors: [String!]!
}

type Fiction implements Book {...}
type NonFiction implements Book {...}

type Query {
  getBookById(id: Int!): Book
}
```

^ We can actually use interfaces to write queries for anything that implements said interface. Without the interface, we would have had to done some workaround to expose all the data, like a getFictionById and a getNonFictionById query, but now we can we simply use the interface to write a singular, generic query

^ There is one gotcha when doing this during implementation. Any type implementing an interface will guaranteed have those fields, but there is no guarantee that the type doesn't have more fields. For this reason, it's important we have some way of instructing GraphQL which type is in play for a given piece of data.

^ Can anyone think of why that's important? Because the client cannot know all aspects of the backend data, and since everything in GraphQL is specifically fetched, knowing the type and introspecting on the API is the most straightforward way to know what to ask for.

---

## Consuming Interfaces

```ruby
interface Book {}
type Fiction implements Book {...}
type NonFiction implements Book {...}
type Query {
  getBooks: [Book]
}

query {
  getBooks {
    title
  }
}
```

^ Querying by interface is much the same as querying by type, namely that since the interface has a declared set of fields, we can simply ask for any fields available in the interface. But there's a problem here, does anyone see it?

^ Right, what if our search result gives us back a Fiction book? It satisfies the list of Book requirement, but we can't possibly know what other fields there are: after all, we only know that we got a list of Books... Just kidding, there is indeed a way to go that next step.

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Consuming Interfaces

```ruby
type Fiction implements Book {
  # Book fields
  genre
}
type NonFiction implements Book {
  # Book fields
  era
}

query {
  getBooks {
    title
  }
}
```

^ If you recall, our Fiction and NonFiction types were very similar, but they had this distinguishing field, genre or era. We need to somehow get those fields from our query. This is where inline fragments come back to play!

---

[.code-highlight: 13-18]

## Interfaces with Fragments

```ruby
type Fiction implements Book {
  # Book fields
  genre
}
type NonFiction implements Book {
  # Book fields
  era
}

query {
  getBooks {
    title
    ... on Fiction {
      genre
    }
    ... on NonFiction {
      era
    }
  }
}
```

^ This dot notation followed by "on Type" is what we call an inline fragment, you'll notice that this looks very similar to the standard Fragment location. Basically, this is like an in-line if operator in our query. This allows us to write generic queries, but also fetch for fields specific to a type. Hopefully, GraphQL is starting to feel pretty thorough!

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Unions

```ruby
type Fiction {
  title: String!
  authors: [String!]!
  genre: String!
}
type NonFiction {
  title: String!
  authors: [String!]!
  era: String!
}

union Book = Fiction | NonFiction
```

^ We're almost done wrapping up our GraphQL basics. Unions are another way of connecting disparate types. Unlike interfaces, there is no exposure requirement, you're just connecting the types. This does reduce the overall amount of code, but it does have one important downside. Can anyone guess?

---

[.code-highlight: 8]

## Consuming Unions

```ruby
type Fiction {...}
type NonFiction {...}

union Book = Fiction | NonFiction

query {
  getBooks {
    __typeName
    ... on Fiction {...}
    ... on NonFiction {...}
  }
}
```

^ The only built-in type we have to explore is this typename field. This is one of the default GraphQL meta fields available to all queries, so we do get to query by that for our union, but that's it. Everything else needs to be queried on the type.

---

## Live Coding!

At this point, if you're reviewing the slides, please check out the example-server branches to see where we're at. You can always refer back to the master branch for the complete code.

---

## Conclusion

^ Conclusion: Hopefully, that was a quick, clean introduction to the basics of GraphQL! We covered a pretty broad set of topics, but these are all things that pretty much every GraphQL server will be using, and things that you would also need to leverage to create a non-trivial server yourself. To recap what you should focus on...

^ All the mechanics behind GraphQL are pretty fascinating, but the most important thing to learn is the concepts of the SDL. That's your contract and it'll affect the ways you construct both the client and the server.

^ Any questions?

---

## Resources

[How To GraphQL](https://www.howtographql.com)
  -> https://www.howtographql.com

[Apollo Server Playground](https://codesandbox.io/s/apollo-server)
  -> https://codesandbox.io/s/apollo-server

[GraphQL Docs](https://graphql.org)
  -> https://graphql.org

[GitHub for Slides/Server](https://github.com/kwak123/graphql-basics)
  -> https://github.com/kwak123/graphql-basics
