---
layout: default
description: If you worked with a relational database management system (RDBMS) such as MySQL,MariaDB or PostgreSQL, you will be familiar with its query language, a dialect of SQL (Structured Query Language)
redirect_from:
  - getting-started-coming-from-sql.html # 3.9 -> 3.10
# TODO: Improve, clarify that this is about the query languages only
---
Coming from SQL
===============

If you work with a relational database management system (RDBMS) such as MySQL,
MariaDB or PostgreSQL, you are familiar with its query language, a dialect
of SQL (Structured Query Language).

ArangoDB's query language is called AQL. There are some similarities between both
languages despite the different data models of the database systems. The most
notable difference is probably the concept of loops in AQL, which makes it feel
more like a programming language. It suits the schema-less model more natural
and makes the query language very powerful while remaining easy to read and write.

To get started with AQL, have a look at our detailed
[comparison of SQL and AQL](https://www.arangodb.com/community-server/sql-aql-comparison/){:target="_blank"}.
It can also help you to translate SQL queries to AQL when migrating to ArangoDB.

{% hint 'info' %}
You may also be interested in the white paper
[**Switching from Relational Databases to ArangoDB**](https://www.arangodb.com/resources/white-paper/coming-from-relational/){:target="_blank"}
available on our website.
{% endhint %}

Basic queries
-------------

**How do select lists translate to AQL queries?**

In traditional SQL, you may either fetch all columns of a table row by row, using
`SELECT * FROM table`, or select a subset of the columns. The list of table
columns to fetch is commonly called *select list*:

```sql
SELECT columnA, columnB, columnZ FROM table
```

Since documents aren't two-dimensional, and you do not want to be limited to
returning two-dimensional lists, the requirements for a query language are higher.
AQL is thus a little bit more complex than plain SQL at first, but offers much
more flexibility in the long run. It lets you handle arbitrarily structured
documents in convenient ways, mostly leaned on the syntax used in JavaScript.

**Composing the documents to be returned**

The AQL `RETURN` statement returns one item per document it is handed. You can
return the whole document, or just parts of it. Given that *oneDocument* is
a document (retrieved like `LET oneDocument = DOCUMENT("myusers/3456789")`
for instance), it can be returned as-is like this:

```aql
RETURN oneDocument
```

```json
[
    {
        "_id": "myusers/3456789",
        "_key": "3456789",
        "_rev": "14253647",
        "firstName": "John",
        "lastName": "Doe",
        "address": {
            "city": "Gotham",
            "street": "Road To Nowhere 1"
        },
        "hobbies": [
            { "name": "swimming", "howFavorite": 10 },
            { "name": "biking", "howFavorite": 6 },
            { "name": "programming", "howFavorite": 4 }
        ]
    }
]
```

Return the hobbies sub-structure only:

```js
RETURN oneDocument.hobbies
```

```json
[
    [
        { "name": "swimming", "howFavorite": 10 },
        { "name": "biking", "howFavorite": 6 },
        { "name": "programming", "howFavorite": 4 }
    ]
]
```

Return the hobbies and the address:

```aql
RETURN {
    hobbies: oneDocument.hobbies,
    address: oneDocument.address
}
```

```json
[
    {
        "hobbies": [
            { "name": "swimming", "howFavorite": 10 },
            { "name": "biking", "howFavorite": 6 },
            { "name": "programming", "howFavorite": 4 }
        ],
        "address": {
            "city": "Gotham",
            "street": "Road To Nowhere 1"
        }
    }
]
```

Return the first hobby only:

```aql
RETURN oneDocument.hobbies[0].name
```

```json
[
    "swimming"
]
```

Return a list of all hobby strings:

```aql
RETURN { hobbies: oneDocument.hobbies[*].name }
```

```json
[
    { "hobbies": ["swimming", "biking", "programming"] }
]
```

More complex [array](aql/functions-array.html) and
[object manipulations](aql/functions-document.html) can be done using
AQL functions and [operators](aql/operators.html).
