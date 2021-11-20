---
sidebar_label: High Level Design
sidebar_position: 20
---
# High Level Design

DefraDBs Query Language (DQL) is used to read, write, modify, and delete data residing within a DefraDB instance. DQL is compliant with the GraphQL specification and uses GraphQL best practices. However, a few divergences from traditional API implementations are necessary since GraphQL was not originally intended to be used as a database query language.







Currently, all query language functionality requires a predefined data schema, using the GraphQL Schema grammar. This will allow us to auto-generate all the necessary types, filters, inputs, payloads, etc to operate the query language with complete type safety guarantees. There may be instances where “Loose Typing” is used in place of Strong Typing, due to the dynamic nature of query languages, and the powerful and expressive features it enables.

Additionally, DefraDBs query language will need to cover 99.99% of developer use cases with respect to interacting with their data layer. This is a notable distinction between this query language, and traditional application GraphQL API endpoints. Traditional API endpoints assume that the developers have the freedom to control their data as they see fit, before returning it through a GraphQL response object. With DefraDB, the binding between GraphQL input/output objects, and the underlying data layer is very tight.

As an example, application GraphQL APIs often enable filtering and aggregation only with respect to their specific application domain. Instead of directly exposing a fully encompassing filter and aggregate layer, they proxy all necessary functionality through custom Query resolvers, that are use case dependent. DefraDB has no such option, since it must provide full functionality and expressiveness directly to the developer. It, has no concept of the domain-specific context the application resides in.

However, DQL still needs to be as developer-friendly as possible, enabling many query language features commonly expected from a data layer.

The major distinction between GraphQL and other database query languages, is the explicit distinction from read-only operations, and write operations, represented as Queries and Mutations respectively. The only hindrance this exposes is the use of subqueries within mutations, commonly found in SQL databases, where one might insert data based on a subquery results. This ideally will be resolved in future versions of DQL.

DQL will make notable use of custom reserved keywords within the GraphQL that will exist only to enable database query language features further. For the most part, reserved keywords will be prefixed by an “_” (underscore) to indicate some kind of special functionality. For example, when filtering or sorting data, the “_lt” (less than) or “_asc” (ascending) keywords will be used to indicate some desired functionality or behavior.

This Query Language specification will try not to make any attempt to rely on implementation details between deployment environments, as DefraDB is intended to operate in several very different deployment environments. If any feature is either deployment dependent, or deviates between implementations, it will be noted as much. Additionally, this specification will try not to indicate best practices (will be covered in another document), and only provide the direct features and references available. This may be most notable when discussing indexes, DefraDB will offer many different types of indexes, each of which has its own pros and cons, and their use is very dependent on the application use case. For completeness, the use of indexes on schemas via directives will be discussed, as well as what features specific indexes provide, but only so far as they relate to the use of them in a Query/Mutation, and not how they might be best used for a particular application.