---
layout: default
description: Users can define their own keys for documents they save
---
Document Keys
=============

Users can define their own keys for documents they save. The document key will
be saved along with a document in the *_key* attribute. Users can pick key
values as required, provided that the values conform to the following
restrictions:

* The key must be a string value. Numeric keys are not allowed, but any numeric
  value can be put into a string and can then be used as document key.
* The key must be at least 1 byte and at most 254 bytes long. Empty keys are 
  disallowed when specified (though it may be valid to completely omit the
  *_key* attribute from a document)
* It must consist of the letters a-z (lower or upper case), the digits 0-9
  or any of the following punctuation characters:
  `_` `-` `:` `.` `@` `(` `)` `+` `,` `=` `;` `$` `!` `*` `'` `%` 
* Any other characters, especially multi-byte UTF-8 sequences, whitespace or 
  punctuation characters cannot be used inside key values
* The key must be unique within the collection it is used

Keys are case-sensitive, i.e. *myKey* and *MyKEY* are considered to be
different keys.

Specifying a document key is optional when creating new documents. If no
document key is specified by the user, ArangoDB will create the document key
itself as each document is required to have a key.

There are no guarantees about the format and pattern of auto-generated document
keys other than the above restrictions. Clients should therefore treat
auto-generated document keys as opaque values and not rely on their format.

The current format for generated keys is a string containing numeric digits.
The numeric values reflect chronological time in the sense that _key values
generated later will contain higher numbers than _key values generated earlier.
But the exact value that will be generated by the server is not predictable.
Note that if you sort on the _key attribute, string comparison will be used,
which means `"100"` is less than `"99"` etc.

{% hint 'info' %}
When working with named graphs, their names are used as document keys in the
`_graphs` system collection. Therefore, the same document key restrictions apply.
{% endhint %}