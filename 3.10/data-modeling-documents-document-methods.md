---
layout: default
description: Collection Methods
---
Collection Methods
==================

All
---

<!-- js/common/modules/@arangodb/arango-collection-common.js-->


`collection.all()`

Fetches all documents from a collection and returns a cursor. You can use
*toArray*, *next*, or *hasNext* to access the result. The result
can be limited using the *skip* and *limit* operator.


**Examples**


Use *toArray* to get all documents at once:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 001_collectionAll
    @EXAMPLE_ARANGOSH_OUTPUT{001_collectionAll}
    ~ db._create("five");
      db.five.insert({ name : "one" });
      db.five.insert({ name : "two" });
      db.five.insert({ name : "three" });
      db.five.insert({ name : "four" });
      db.five.insert({ name : "five" });
      db.five.all().toArray();
    ~ db._drop("five");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 001_collectionAll
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Use *limit* to restrict the documents:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 002_collectionAllNext
    @EXAMPLE_ARANGOSH_OUTPUT{002_collectionAllNext}
    ~ db._create("five");
      db.five.insert({ name : "one" });
      db.five.insert({ name : "two" });
      db.five.insert({ name : "three" });
      db.five.insert({ name : "four" });
      db.five.insert({ name : "five" });
      db.five.all().limit(2).toArray();
    ~ db._drop("five");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 002_collectionAllNext
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


Query by example
----------------

<!-- js/common/modules/@arangodb/arango-collection-common.js-->


`collection.byExample(example)`

Fetches all documents from a collection that match the specified
example and returns a cursor.

You can use *toArray*, *next*, or *hasNext* to access the
result. The result can be limited using the *skip* and *limit*
operator.

An attribute name of the form *a.b* is interpreted as attribute path,
not as attribute. If you use

```json
{ "a" : { "c" : 1 } }
```

as example, then you will find all documents, such that the attribute
*a* contains a document of the form *{c : 1 }*. For example the document

```json
{ "a" : { "c" : 1 }, "b" : 1 }
```

will match, but the document

```json
{ "a" : { "c" : 1, "b" : 1 } }
```

will not.

However, if you use

```json
{ "a.c" : 1 }
```

then you will find all documents, which contain a sub-document in *a*
that has an attribute *c* of value *1*. Both the following documents

```json
{ "a" : { "c" : 1 }, "b" : 1 }
```

and

```json
{ "a" : { "c" : 1, "b" : 1 } }
```

will match.

```js
collection.byExample(path1, value1, ...)
```

As alternative you can supply an array of paths and values.

**Examples**

Use *toArray* to get all documents at once:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 003_collectionByExample
    @EXAMPLE_ARANGOSH_OUTPUT{003_collectionByExample}
    ~ db._create("users");
      db.users.insert({ name: "Gerhard" });
      db.users.insert({ name: "Helmut" });
      db.users.insert({ name: "Angela" });
      db.users.all().toArray();
      db.users.byExample({ "_id" : "users/20" }).toArray();
      db.users.byExample({ "name" : "Gerhard" }).toArray();
      db.users.byExample({ "name" : "Helmut", "_id" : "users/15" }).toArray();
    ~ db._drop("users");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 003_collectionByExample
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Use *next* to loop over all documents:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 004_collectionByExampleNext
    @EXAMPLE_ARANGOSH_OUTPUT{004_collectionByExampleNext}
    ~ db._create("users");
      db.users.insert({ name: "Gerhard" });
      db.users.insert({ name: "Helmut" });
      db.users.insert({ name: "Angela" });
      var a = db.users.byExample( {"name" : "Angela" } );
      while (a.hasNext()) print(a.next());
    ~ db._drop("users");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 004_collectionByExampleNext
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

First Example
-------------

<!-- js/server/modules/@arangodb/arango-collection.js-->


`collection.firstExample(example)`

Returns some document of a collection that matches the specified
example. If no such document exists, *null* will be returned.
The example has to be specified as paths and values.
See *byExample* for details.

`collection.firstExample(path1, value1, ...)`

As alternative you can supply an array of paths and values.


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline collectionFirstExample
    @EXAMPLE_ARANGOSH_OUTPUT{collectionFirstExample}
    ~ db._create("users");
    ~ db.users.insert({ name: "Gerhard" });
    ~ db.users.insert({ name: "Helmut" });
    ~ db.users.insert({ name: "Angela" });
      db.users.firstExample("name", "Angela");
    ~ db._drop("users");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock collectionFirstExample
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Any
---

<!-- js/server/modules/@arangodb/arango-collection.js-->


`collection.any()`

Returns a random document from the collection or *null* if none exists.

**Note**: this method is expensive when using the RocksDB storage engine.

Count
-----

<!-- arangod/V8Server/v8-vocbase.cpp -->


`collection.count()`

Returns the number of living documents in the collection.


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline collectionCount
    @EXAMPLE_ARANGOSH_OUTPUT{collectionCount}
    ~ db._create("users");
      db.users.count();
    ~ db._drop("users");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock collectionCount
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


toArray
-------

<!-- js/server/modules/@arangodb/arango-collection.js-->


`collection.toArray()`

Converts the collection into an array of documents. Never use this call
in a production environment as it will basically create a copy of your
collection in RAM which will use resources depending on the number and size
of the documents in your collecion.


Document
--------

<!-- arangod/V8Server/v8-vocbase.cpp -->


`collection.document(object [, options])`

The *document* method finds a document given an object *object*
containing the *_id* or *_key* attribute. The method returns
the document if it can be found. If both attributes are given,
the *_id* takes precedence, it is an error, if the collection part
of the *_id* does not match the *collection*.

An error is thrown if *_rev* is specified but the document found has a
different revision already. An error is also thrown if no document exists
with the given *_id* or *_key* value.

Please note that if the method is executed on the arangod server (e.g. from
inside a Foxx application), an immutable document object will be returned
for performance reasons. It is not possible to change attributes of this
immutable object. To update or patch the returned document, it needs to be
cloned/copied into a regular JavaScript object first. This is not necessary
if the *document* method is called from out of arangosh or from any other
client.

If you pass *options* as the second argument, it must be an object. If this
object has the `allowDirtyReads` attribute set to `true`, then the
Coordinator is allowed to read from any shard replica and not only from
the leader. See [Read from Followers](http/document-address-and-etag.html#read-from-followers)
for details.

`collection.document(document-handle [, options])`

As before. Instead of *object* a *document-handle* can be passed as
first argument. No revision can be specified in this case.

`collection.document(document-key [, options])`

As before. Instead of *object* a *document-key* can be passed as
first argument.

`collection.document(array [, options])`

This variant allows to perform the operation on a whole array of arguments.
The behavior is exactly as if *document* would have been called on all members
of the array separately and all results are returned in an array. If an error
occurs with any of the documents, no exception is risen! Instead of a document
an error object is returned in the result array.

*Examples*

Returns the document for a document-handle:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameValidPlain
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameValidPlain}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "2873916"});
      db.example.document("example/2873916");
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameValidPlain
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Returns the document for a document-key:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameValidByKey
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameValidByKey}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "2873916"});
      db.example.document("2873916");
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameValidByKey
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Returns the document for an object:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameValidByObject
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameValidByObject}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "2873916"});
      db.example.document({_id: "example/2873916"});
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameValidByObject
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Returns the document for an array of two keys:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameValidMulti
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameValidMulti}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "2873916"});
    ~ var myid = db.example.insert({_key: "2873917"});
      db.example.document(["2873916","2873917"]);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameValidMulti
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

An error is raised if the document is unknown:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameUnknown
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameUnknown}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "2873916"});
      db.example.document("example/4472917"); // xpError(ERROR_ARANGO_DOCUMENT_NOT_FOUND)
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameUnknown
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

An error is raised if the handle is invalid:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionNameHandle
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionNameHandle}
    ~ db._create("example");
      db.example.document(""); // xpError(ERROR_ARANGO_DOCUMENT_HANDLE_BAD)
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionNameHandle
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


Exists
------

<!-- arangod/V8Server/v8-vocbase.cpp -->


checks whether a document exists
`collection.exists(object [, options])`

The *exists* method determines whether a document exists given an object
`object` containing the *_id* or *_key* attribute. If both attributes
are given, the *_id* takes precedence, it is an error, if the collection
part of the *_id* does not match the *collection*.

An error is thrown if *_rev* is specified but the document found has a
different revision already.

Instead of returning the found document or an error, this method will
only return an object with the attributes *_id*, *_key* and *_rev*, or
*false* if no document with the given *_id* or *_key* exists. It can
thus be used for easy existence checks.

This method will throw an error if used improperly, e.g. when called
with a non-document handle, a non-document, or when a cross-collection
request is performed.

If you pass *options* as the second argument, it must be an object. If this
object has the `allowDirtyReads` attribute set to `true`, then the
Coordinator is allowed to read from any shard replica and not only from
the leader. See [Read from Followers](http/document-address-and-etag.html#read-from-followers)
for details.

`collection.exists(document-handle [, options])`

As before. Instead of *object* a *document-handle* can be passed as
first argument.

`collection.exists(document-key [, options])`

As before. Instead of *object* a *document-key* can be passed as
first argument.

`collection.exists(array [, options])`

This variant allows to perform the operation on a whole array of arguments.
The behavior is exactly as if *exists* would have been called on all
members of the array separately and all results are returned in an array. If an error
occurs with any of the documents, the operation stops immediately returning
only an error object.


Lookup By Keys
--------------

<!-- arangod/V8Server/v8-query.cpp-->


`collection.documents(keys)`

Looks up the documents in the specified collection using the array of
keys provided. All documents for which a matching key was specified in
the *keys* array and that exist in the collection will be returned. Keys
for which no document can be found in the underlying collection are
ignored, and no exception will be thrown for them.

This method is deprecated in favour of the array variant of *document*.

**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline collectionLookupByKeys
    @EXAMPLE_ARANGOSH_OUTPUT{collectionLookupByKeys}
    ~ db._drop("example");
    ~ db._create("example");
      keys = [ ];
    | for (var i = 0; i < 10; ++i) {
    |   db.example.insert({ _key: "test" + i, value: i });
    |   keys.push("test" + i);
      }
      db.example.documents(keys);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock collectionLookupByKeys
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Insert / Save
-------------

<!-- arangod/V8Server/v8-vocbase.cpp -->

`collection.insert(data)`<br>
`collection.save(data)`

Creates a new document in the *collection* from the given *data*. The
*data* must be an object. The attributes *_id* and *_rev* are ignored
and are automatically generated. A unique value for the attribute *_key*
will be automatically generated if not specified. If specified, there
must not be a document with the given *_key* in the collection.

The method returns a document with the attributes *_id*, *_key* and
*_rev*. The attribute *_id* contains the document handle of the newly
created document, the attribute *_key* the document key and the
attribute *_rev* contains the document revision.

`collection.insert(data, options)`<br>
`collection.save(data, options)`

Creates a new document in the *collection* from the given *data* as
above. The optional *options* parameter must be an object and can be
used to specify the following options:

  - *waitForSync*: One can force
    synchronization of the document creation operation to disk even in
    case that the *waitForSync* flag is been disabled for the entire
    collection. Thus, the *waitForSync* option can be used to force
    synchronization of just specific operations. To use this, set the
    *waitForSync* parameter to *true*. If the *waitForSync* parameter
    is not specified or set to *false*, then the collection's default
    *waitForSync* behavior is applied. The *waitForSync* parameter
    cannot be used to disable synchronization for collections that have
    a default *waitForSync* value of *true*.
  - *silent*: If this flag is set to *true*, the method does not return
    any output.
  - *returnNew*: If this flag is set to *true*, the complete new document
    is returned in the output under the attribute *new*.
  - *returnOld*: If this flag is set to *true*, the complete old document
    is returned in the output under the attribute *old*. Only available 
    in combination with the *overwrite* option
  - *overwrite*: If set to *true*, the insert becomes a replace-insert.
    If a document with the same *_key* exists already the new document
    is not rejected with unique constraint violated but will replace
    the old document. Note that operations with `overwrite` parameter require
    a `_key` attribute in the request payload, therefore they can only be
    performed on collections sharded by `_key`.
  - *overwriteMode*: this optional flag can have one of the following values:
    - *ignore*: if a document with the specified *_key* value exists already,
      nothing will be done and no write operation will be carried out
      (introduced in v3.7.0).
      The insert operation will return success in this case. This mode does not
      support returning the old document version using the *returnOld*
      attribute. *returnNew* will only set the *new* attribute in the response
      if a new document was inserted.
    - *replace*: if a document with the specified *_key* value exists already,
      it will be overwritten with the specified document value. This mode will
      also be used when no overwrite mode is specified but the *overwrite*
      flag is set to *true*.
    - *update*: if a document with the specified *_key* value exists already,
      it will be patched (partially updated) with the specified document value
      (introduced in v3.7.0).
      The overwrite mode can be further controlled via the *keepNull* and
      *mergeObjects* parameters.
    - *conflict*: if a document with the specified *_key* value exists already,
      return a unique constraint violation error so that the insert operation
      fails. This is also the default behavior in case the overwrite mode is
      not set, and the *overwrite* flag is *false* or not set either.
  - *keepNull*: The optional *keepNull* parameter can be used to modify
    the behavior when handling *null* values. Normally, *null* values
    are stored in the database. By setting the *keepNull* parameter to
    *false*, this behavior can be changed so that all attributes in
    *data* with *null* values will be removed from the target document.
    This option controls the update-insert behavior only.
  - *mergeObjects*: Controls whether objects (not arrays) will be
    merged if present in both the existing and the patch document. If
    set to *false*, the value in the patch document will overwrite the
    existing document's value. If set to *true*, objects will be merged.
    The default is *true*.
    This option controls the update-insert behavior only.

`collection.insert(array)`

`collection.insert(array, options)`

These two variants allow to perform the operation on a whole array of
arguments. The behavior is exactly as if *insert* would have been called on all
members of the array separately and all results are returned in an array. If an
error occurs with any of the documents, no exception is risen! Instead of a
document an error object is returned in the result array. The options behave
exactly as before.

**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionInsertSingle
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionInsertSingle}
    ~ db._create("example");
      db.example.insert({ Hello : "World" });
      db.example.insert({ Hello : "World" }, {waitForSync: true});
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionInsertSingle
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionInsertMulti
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionInsertMulti}
    ~ db._create("example");
      db.example.insert([{ Hello : "World" }, {Hello: "there"}])
      db.example.insert([{ Hello : "World" }, {}], {waitForSync: true});
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionInsertMulti
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionInsertSingleOverwrite
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionInsertSingleOverwrite}
    ~ db._create("example");
      db.example.insert({ _key : "666", Hello : "World" });
      db.example.insert({ _key : "666", Hello : "Universe" }, {overwrite: true, returnOld: true});
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionInsertSingleOverwrite
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Replace
-------

<!-- arangod/V8Server/v8-vocbase.cpp -->


`collection.replace(selector, data)`

Replaces an existing document described by the *selector*, which must
be an object containing the *_id* or *_key* attribute. There must be
a document with that *_id* or *_key* in the current collection. This
document is then replaced with the *data* given as second argument.
Any attribute *_id*, *_key* or *_rev* in *data* is ignored.

The method returns a document with the attributes *_id*, *_key*, *_rev*
and *_oldRev*. The attribute *_id* contains the document handle of the
updated document, the attribute *_rev* contains the document revision of
the updated document, the attribute *_oldRev* contains the revision of
the old (now replaced) document.

If the selector contains a *_rev* attribute, the method first checks
that the specified revision is the current revision of that document.
If not, there is a conflict, and an error is thrown.

`collection.replace(selector, data, options)`

As before, but *options* must be an object that can contain the following
boolean attributes:

  - *waitForSync*: One can force
    synchronization of the document creation operation to disk even in
    case that the *waitForSync* flag is been disabled for the entire
    collection. Thus, the *waitForSync* option can be used to force
    synchronization of just specific operations. To use this, set the
    *waitForSync* parameter to *true*. If the *waitForSync* parameter
    is not specified or set to *false*, then the collection's default
    *waitForSync* behavior is applied. The *waitForSync* parameter
    cannot be used to disable synchronization for collections that have
    a default *waitForSync* value of *true*.
  - *overwrite*: If this flag is set to *true*, a *_rev* attribute in
    the selector is ignored.
  - *returnNew*: If this flag is set to *true*, the complete new document
    is returned in the output under the attribute *new*.
  - *returnOld*: If this flag is set to *true*, the complete previous
    revision of the document is returned in the output under the
    attribute *old*.
  - *silent*: If this flag is set to *true*, no output is returned.

`collection.replace(document-handle, data)`

`collection.replace(document-handle, data, options)`

As before. Instead of *selector* a *document-handle* can be passed as
first argument. No revision precondition is tested.

`collection.replace(document-key, data)`

`collection.replace(document-key, data, options)`

As before. Instead of *selector* a *document-key* can be passed as
first argument. No revision precondition is tested.

`collection.replace(selectorarray, dataarray)`

`collection.replace(selectorarray, dataarray, options)`

These two variants allow to perform the operation on a whole array of
selector/data pairs. The two arrays given as *selectorarray* and *dataarray*
must have the same length. The behavior is exactly as if *replace* would have
been called on all respective members of the two arrays and all results are
returned in an array. If an error occurs with any of the documents, no
exception is risen! Instead of a document an error object is returned in the
result array. The options behave exactly as before.
 
**Examples**


Create and update a document:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionReplace1
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionReplace1}
    ~ db._create("example");
      a1 = db.example.insert({ a : 1 });
      a2 = db.example.replace(a1, { a : 2 });
      a3 = db.example.replace(a1, { a : 3 }); // xpError(ERROR_ARANGO_CONFLICT);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionReplace1
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Use a document handle:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollectionReplaceHandle
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollectionReplaceHandle}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "3903044"});
      a1 = db.example.insert({ a : 1 });
      a2 = db.example.replace("example/3903044", { a : 2 });
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollectionReplaceHandle
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


Update
------

<!-- arangod/V8Server/v8-vocbase.cpp -->

`collection.update(selector, data)`

Updates an existing document described by the *selector*, which must
be an object containing the *_id* or *_key* attribute. There must be
a document with that *_id* or *_key* in the current collection. This
document is then patched with the *data* given as second argument.
Any attribute *_id*, *_key* or *_rev* in *data* is ignored.

The method returns a document with the attributes *_id*, *_key*, *_rev*
and *_oldRev*. The attribute *_id* contains the document handle of the
updated document, the attribute *_rev* contains the document revision of
the updated document, the attribute *_oldRev* contains the revision of
the old (now updated) document.

If the selector contains a *_rev* attribute, the method first checks
that the specified revision is the current revision of that document.
If not, there is a conflict, and an error is thrown.

`collection.update(selector, data, options)`

As before, but *options* must be an object that can contain the following
boolean attributes:

  - *waitForSync*: One can force
    synchronization of the document creation operation to disk even in
    case that the *waitForSync* flag is been disabled for the entire
    collection. Thus, the *waitForSync* option can be used to force
    synchronization of just specific operations. To use this, set the
    *waitForSync* parameter to *true*. If the *waitForSync* parameter
    is not specified or set to *false*, then the collection's default
    *waitForSync* behavior is applied. The *waitForSync* parameter
    cannot be used to disable synchronization for collections that have
    a default *waitForSync* value of *true*.
  - *overwrite*: If this flag is set to *true*, a *_rev* attribute in
    the selector is ignored.
  - *returnNew*: If this flag is set to *true*, the complete new document
    is returned in the output under the attribute *new*.
  - *returnOld*: If this flag is set to *true*, the complete previous
    revision of the document is returned in the output under the
    attribute *old*.
  - *silent*: If this flag is set to *true*, no output is returned.
  - *keepNull*: The optional *keepNull* parameter can be used to modify
    the behavior when handling *null* values. Normally, *null* values
    are stored in the database. By setting the *keepNull* parameter to
    *false*, this behavior can be changed so that all attributes in
    *data* with *null* values will be removed from the target document.
  - *mergeObjects*: Controls whether objects (not arrays) will be
    merged if present in both the existing and the patch document. If
    set to *false*, the value in the patch document will overwrite the
    existing document's value. If set to *true*, objects will be merged.
    The default is *true*.


`collection.update(document-handle, data)`

`collection.update(document-handle, data, options)`

As before. Instead of *selector* a *document-handle* can be passed as
first argument. No revision precondition is tested.

`collection.update(document-key, data)`

`collection.update(document-key, data, options)`

As before. Instead of *selector* a *document-key* can be passed as
first argument. No revision precondition is tested.

`collection.update(selectorarray, dataarray)`

`collection.update(selectorarray, dataarray, options)`

These two variants allow to perform the operation on a whole array of
selector/data pairs. The two arrays given as *selectorarray* and *dataarray*
must have the same length. The behavior is exactly as if *update* would have
been called on all respective members of the two arrays and all results are
returned in an array. If an error occurs with any of the documents, no
exception is risen! Instead of a document an error object is returned in the
result array. The options behave exactly as before.

*Examples*

Create and update a document:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollection_UpdateDocument
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollection_UpdateDocument}
    ~ db._create("example");
      a1 = db.example.insert({"a" : 1});
      a2 = db.example.update(a1, {"b" : 2, "c" : 3});
      a3 = db.example.update(a1, {"d" : 4}); // xpError(ERROR_ARANGO_CONFLICT);
      a4 = db.example.update(a2, {"e" : 5, "f" : 6 });
      db.example.document(a4);
      a5 = db.example.update(a4, {"a" : 1, c : 9, e : 42 });
      db.example.document(a5);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollection_UpdateDocument
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Use a document handle:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollection_UpdateHandleSingle
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollection_UpdateHandleSingle}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "18612115"});
      a1 = db.example.insert({"a" : 1});
      a2 = db.example.update("example/18612115", { "x" : 1, "y" : 2 });
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollection_UpdateHandleSingle
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Use the keepNull parameter to remove attributes with null values:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollection_UpdateHandleKeepNull
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollection_UpdateHandleKeepNull}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "19988371"});
      db.example.insert({"a" : 1});
    |db.example.update("example/19988371",
                       { "b" : null, "c" : null, "d" : 3 });
      db.example.document("example/19988371");
      db.example.update("example/19988371", { "a" : null }, false, false);
      db.example.document("example/19988371");
    | db.example.update("example/19988371",
                        { "b" : null, "c": null, "d" : null }, false, false);
      db.example.document("example/19988371");
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollection_UpdateHandleKeepNull
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Patching array values:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentsCollection_UpdateHandleArray
    @EXAMPLE_ARANGOSH_OUTPUT{documentsCollection_UpdateHandleArray}
    ~ db._create("example");
    ~ var myid = db.example.insert({_key: "20774803"});
    |  db.example.insert({"a" : { "one" : 1, "two" : 2, "three" : 3 },
                          "b" : { }});
    | db.example.update("example/20774803", {"a" : { "four" : 4 },
                                             "b" : { "b1" : 1 }});
      db.example.document("example/20774803");
    | db.example.update("example/20774803", { "a" : { "one" : null },
    |                                         "b" : null },
                        false, false);
      db.example.document("example/20774803");
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentsCollection_UpdateHandleArray
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


Remove
------

<!-- arangod/V8Server/v8-vocbase.cpp -->

`collection.remove(selector)`

Removes a document described by the *selector*, which must be an object
containing the *_id* or *_key* attribute. There must be a document with
that *_id* or *_key* in the current collection. This document is then
removed.

The method returns a document with the attributes *_id*, *_key* and *_rev*.
The attribute *_id* contains the document handle of the
removed document, the attribute *_rev* contains the document revision of
the removed document.

If the selector contains a *_rev* attribute, the method first checks
that the specified revision is the current revision of that document.
If not, there is a conflict, and an error is thrown.

`collection.remove(selector, options)`

As before, but *options* must be an object that can contain the following
boolean attributes:

  - *waitForSync*: One can force
    synchronization of the document creation operation to disk even in
    case that the *waitForSync* flag is been disabled for the entire
    collection. Thus, the *waitForSync* option can be used to force
    synchronization of just specific operations. To use this, set the
    *waitForSync* parameter to *true*. If the *waitForSync* parameter
    is not specified or set to *false*, then the collection's default
    *waitForSync* behavior is applied. The *waitForSync* parameter
    cannot be used to disable synchronization for collections that have
    a default *waitForSync* value of *true*.
  - *overwrite*: If this flag is set to *true*, a *_rev* attribute in
    the selector is ignored.
  - *returnOld*: If this flag is set to *true*, the complete previous
    revision of the document is returned in the output under the
    attribute *old*.
  - *silent*: If this flag is set to *true*, no output is returned.

`collection.remove(document-handle)`

`collection.remove(document-handle, options)`

As before. Instead of *selector* a *document-handle* can be passed as
first argument. No revision check is performed.

`collection.remove(document-key)`

`collection.remove(document-handle, options)`

As before. Instead of *selector* a *document-handle* can be passed as
first argument. No revision check is performed.

`collection.remove(selectorarray)`

`collection.remove(selectorarray,options)`

These two variants allow to perform the operation on a whole array of
selectors. The behavior is exactly as if *remove* would have been called on all
members of the array separately and all results are returned in an array. If an
error occurs with any of the documents, no exception is risen! Instead of a
document an error object is returned in the result array. The options behave
exactly as before.

**Examples**


Remove a document:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentDocumentRemoveSimple
    @EXAMPLE_ARANGOSH_OUTPUT{documentDocumentRemoveSimple}
    ~ db._create("example");
      a1 = db.example.insert({ a : 1 });
      db.example.document(a1);
      db.example.remove(a1);
      db.example.document(a1); // xpError(ERROR_ARANGO_DOCUMENT_NOT_FOUND);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentDocumentRemoveSimple
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Remove a document with a conflict:

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline documentDocumentRemoveConflict
    @EXAMPLE_ARANGOSH_OUTPUT{documentDocumentRemoveConflict}
    ~ db._create("example");
      a1 = db.example.insert({ a : 1 });
      a2 = db.example.replace(a1, { a : 2 });
      db.example.remove(a1);       // xpError(ERROR_ARANGO_CONFLICT);
      db.example.remove(a1, true);
      db.example.document(a1);     // xpError(ERROR_ARANGO_DOCUMENT_NOT_FOUND);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock documentDocumentRemoveConflict
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}


Remove By Keys
--------------

<!-- arangod/V8Server/v8-query.cpp-->


`collection.removeByKeys(keys)`


Looks up the documents in the specified collection using the array of keys
provided, and removes all documents from the collection whose keys are
contained in the *keys* array. Keys for which no document can be found in
the underlying collection are ignored, and no exception will be thrown for
them.

The method will return an object containing the number of removed documents
in the *removed* sub-attribute, and the number of not-removed/ignored
documents in the *ignored* sub-attribute.

This method is deprecated in favour of the array variant of *remove*.

**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline collectionRemoveByKeys
    @EXAMPLE_ARANGOSH_OUTPUT{collectionRemoveByKeys}
    ~ db._drop("example");
    ~ db._create("example");
      keys = [ ];
    | for (var i = 0; i < 10; ++i) {
    |   db.example.insert({ _key: "test" + i, value: i });
    |   keys.push("test" + i);
      }
      db.example.removeByKeys(keys);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock collectionRemoveByKeys
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Remove By Example
-----------------

<!-- js/common/modules/@arangodb/arango-collection-common.js-->


`collection.removeByExample(example)`

Removes all documents matching an example.

`collection.removeByExample(document, waitForSync)`

The optional *waitForSync* parameter can be used to force synchronization
of the document deletion operation to disk even in case that the
*waitForSync* flag had been disabled for the entire collection. Thus,
the *waitForSync* parameter can be used to force synchronization of just
specific operations. To use this, set the *waitForSync* parameter to
*true*. If the *waitForSync* parameter is not specified or set to
*false*, then the collection's default *waitForSync* behavior is
applied. The *waitForSync* parameter cannot be used to disable
synchronization for collections that have a default *waitForSync* value
of *true*.

`collection.removeByExample(document, waitForSync, limit)`

The optional *limit* parameter can be used to restrict the number of
removals to the specified value. If *limit* is specified but less than the
number of documents in the collection, it is undefined which documents are
removed.


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 010_documentsCollectionRemoveByExample
    @EXAMPLE_ARANGOSH_OUTPUT{010_documentsCollectionRemoveByExample}
    ~ db._create("example");
    ~ db.example.insert({ Hello : "world" });
      db.example.removeByExample( {Hello : "world"} );
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 010_documentsCollectionRemoveByExample
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Replace By Example
------------------

<!-- js/common/modules/@arangodb/arango-collection-common.js-->


`collection.replaceByExample(example, newValue)`

Replaces all documents matching an example with a new document body.
The entire document body of each document matching the *example* will be
replaced with *newValue*. The document meta-attributes *_id*, *_key* and
*_rev* will not be replaced.

`collection.replaceByExample(document, newValue, waitForSync)`

The optional *waitForSync* parameter can be used to force synchronization
of the document replacement operation to disk even in case that the
*waitForSync* flag had been disabled for the entire collection. Thus,
the *waitForSync* parameter can be used to force synchronization of just
specific operations. To use this, set the *waitForSync* parameter to
*true*. If the *waitForSync* parameter is not specified or set to
*false*, then the collection's default *waitForSync* behavior is
applied. The *waitForSync* parameter cannot be used to disable
synchronization for collections that have a default *waitForSync* value
of *true*.

`collection.replaceByExample(document, newValue, waitForSync, limit)`

The optional *limit* parameter can be used to restrict the number of
replacements to the specified value. If *limit* is specified but less than
the number of documents in the collection, it is undefined which documents are
replaced.


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 011_documentsCollectionReplaceByExample
    @EXAMPLE_ARANGOSH_OUTPUT{011_documentsCollectionReplaceByExample}
    ~ db._create("example");
      db.example.insert({ Hello : "world" });
      db.example.replaceByExample({ Hello: "world" }, {Hello: "mars"}, false, 5);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 011_documentsCollectionReplaceByExample
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Update By Example
-----------------

<!-- js/common/modules/@arangodb/arango-collection-common.js-->


`collection.updateByExample(example, newValue)`

Partially updates all documents matching an example with a new document body.
Specific attributes in the document body of each document matching the
*example* will be updated with the values from *newValue*.
The document meta-attributes *_id*, *_key* and *_rev* cannot be updated.

Partial update could also be used to append new fields,
if there were no old field with same name.

`collection.updateByExample(document, newValue, keepNull, waitForSync)`

The optional *keepNull* parameter can be used to modify the behavior when
handling *null* values. Normally, *null* values are stored in the
database. By setting the *keepNull* parameter to *false*, this behavior
can be changed so that all attributes in *data* with *null* values will
be removed from the target document.

The optional *waitForSync* parameter can be used to force synchronization
of the document replacement operation to disk even in case that the
*waitForSync* flag had been disabled for the entire collection. Thus,
the *waitForSync* parameter can be used to force synchronization of just
specific operations. To use this, set the *waitForSync* parameter to
*true*. If the *waitForSync* parameter is not specified or set to
*false*, then the collection's default *waitForSync* behavior is
applied. The *waitForSync* parameter cannot be used to disable
synchronization for collections that have a default *waitForSync* value
of *true*.

`collection.updateByExample(document, newValue, keepNull, waitForSync, limit)`

The optional *limit* parameter can be used to restrict the number of
updates to the specified value. If *limit* is specified but less than
the number of documents in the collection, it is undefined which documents are
updated.

`collection.updateByExample(document, newValue, options)`

Using this variant, the options for the operation can be passed using
an object with the following sub-attributes:

  - *keepNull*
  - *waitForSync*
  - *limit*
  - *mergeObjects*


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline 012_documentsCollectionUpdateByExample
    @EXAMPLE_ARANGOSH_OUTPUT{012_documentsCollectionUpdateByExample}
    ~ db._create("example");
      db.example.insert({ Hello : "world", foo : "bar" });
      db.example.updateByExample({ Hello: "world" }, { Hello: "foo", World: "bar" }, false);
      db.example.byExample({ Hello: "foo" }).toArray()
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock 012_documentsCollectionUpdateByExample
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Collection type
---------------

`collection.type()`

Returns the type of a collection. Possible values are:
- 2: document collection
- 3: edge collection


Convert a document key to a document id
---------------------------------------

<!-- js/common/modules/@arangodb/arango-collection-common.js-->

`collection.documentId(documentKey)`

Qualifies the given document key with this collection's name to derive a
valid document id.

Throws if the document key is invalid. Note that this method does not
check whether the document already exists in this collection.


Edges
-----

Edges are normal documents that always contain a `_from` and a `_to`
attribute. Therefore, you can use the document methods to operate on
edges. The following methods, however, are specific to edges.

`edge-collection.edges(vertex)`

The *edges* operator finds all edges starting from (outbound) or ending
in (inbound) *vertex*.

`edge-collection.edges(vertices)`

The *edges* operator finds all edges starting from (outbound) or ending
in (inbound) a document from *vertices*, which must be a list of documents
or document handles.

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline EDGCOL_02_Relation
    @EXAMPLE_ARANGOSH_OUTPUT{EDGCOL_02_Relation}
      db._create("vertex");
      db._createEdgeCollection("relation");
      var myGraph = {};
      myGraph.v1 = db.vertex.insert({ name : "vertex 1" });
      myGraph.v2 = db.vertex.insert({ name : "vertex 2" });
    | myGraph.e1 = db.relation.insert(myGraph.v1, myGraph.v2,
                                      { label : "knows"});
      db._document(myGraph.e1);
      db.relation.edges(myGraph.e1._id);
    ~ db._drop("relation");
    ~ db._drop("vertex");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock EDGCOL_02_Relation
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

`edge-collection.inEdges(vertex)`

The *edges* operator finds all edges ending in (inbound) *vertex*.

`edge-collection.inEdges(vertices)`

The *edges* operator finds all edges ending in (inbound) a document from
*vertices*, which must a list of documents or document handles.

**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline EDGCOL_02_inEdges
    @EXAMPLE_ARANGOSH_OUTPUT{EDGCOL_02_inEdges}
      db._create("vertex");
      db._createEdgeCollection("relation");
    ~ var myGraph = {};
      myGraph.v1 = db.vertex.insert({ name : "vertex 1" });
      myGraph.v2 = db.vertex.insert({ name : "vertex 2" });
    | myGraph.e1 = db.relation.insert(myGraph.v1, myGraph.v2,
                                      { label : "knows"});
      db._document(myGraph.e1);
      db.relation.inEdges(myGraph.v1._id);
      db.relation.inEdges(myGraph.v2._id);
    ~ db._drop("relation");
    ~ db._drop("vertex");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock EDGCOL_02_inEdges
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

`edge-collection.outEdges(vertex)`

The *edges* operator finds all edges starting from (outbound)
*vertices*.

`edge-collection.outEdges(vertices)`

The *edges* operator finds all edges starting from (outbound) a document
from *vertices*, which must a list of documents or document handles.


**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline EDGCOL_02_outEdges
    @EXAMPLE_ARANGOSH_OUTPUT{EDGCOL_02_outEdges}
      db._create("vertex");
      db._createEdgeCollection("relation");
    ~ var myGraph = {};
      myGraph.v1 = db.vertex.insert({ name : "vertex 1" });
      myGraph.v2 = db.vertex.insert({ name : "vertex 2" });
    | myGraph.e1 = db.relation.insert(myGraph.v1, myGraph.v2,
                                      { label : "knows"});
      db._document(myGraph.e1);
      db.relation.outEdges(myGraph.v1._id);
      db.relation.outEdges(myGraph.v2._id);
    ~ db._drop("relation");
    ~ db._drop("vertex");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock EDGCOL_02_outEdges
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}

Misc
----

`collection.iterate(iterator, options)`

Iterates over some elements of the collection and apply the function
*iterator* to the elements. The function will be called with the
document as first argument and the current number (starting with 0)
as second argument.

*options* must be an object with the following attributes:

  - *limit* (optional, default none): use at most *limit* documents.

  - *probability* (optional, default all): a number between *0* and
    *1*. Documents are chosen with this probability.

**Examples**

{% arangoshexample examplevar="examplevar" script="script" result="result" %}
    @startDocuBlockInline accessViaGeoIndex
    @EXAMPLE_ARANGOSH_OUTPUT{accessViaGeoIndex}
    ~db._create("example")
    |for (i = -90;  i <= 90;  i += 10) {
    |  for (j = -180;  j <= 180;  j += 10) {
    |    db.example.insert({ name : "Name/" + i + "/" + j,
    |                      home : [ i, j ],
    |                      work : [ -i, -j ] });
    |  }
    |}

     db.example.ensureIndex({ type: "geo", fields: [ "home" ] });
     |items = db.example.getIndexes().map(function(x) { return x.id; });
     db.example.index(items[1]);
    ~ db._drop("example");
    @END_EXAMPLE_ARANGOSH_OUTPUT
    @endDocuBlock accessViaGeoIndex
{% endarangoshexample %}
{% include arangoshexample.html id=examplevar script=script result=result %}
