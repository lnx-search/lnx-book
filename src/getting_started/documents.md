# Documents

Documents can only be added or removed, not updated.
This is preferred to be done explicitly hence no update endpoint.

### Endpoints Required
Adds one or more documents to the given index.
This can either return immediately or wait for all operations 
to be submitted depending on the `wait` query parameter.
```
POST /indexes/:index_name/documents?wait=<bool>
```

Deletes any documents matching the set of given terms. (Requires a JSON body.)
```
DELETE /indexes/:index_name/documents
```

Deleting all documents (This requires no payload): 
```
DELETE /indexes/:index_name/documents/clear
```

## Adding a document
Adding a document is relatively simple, you can either add a single
document represented as a JSON object or you can submit a array of object.

Every document is checked for the required fields, if any docs are missing
fields the *entire* request is rejected.

```
POST /indexes/:index_name/documents?wait=<bool>
```

If `wait=true` (recommended) the system will wait for all operations (one operation per doc)
to be submitted before returning. The writer has a 20 operation buffer limit, this can be
useful for client backpressure waiting for when to send more documents or not.

Not waiting can quickly add a lot of load on the server which may be un-desirable.

#### Representing Data Types
*Most* data types can be represented as raw 

#### Example Body (Singular)
```js
{
    "title": ["Hello, World"],
    "description": ["Welcome to the next generation system."]
}
```

#### Example Body (Multiple)
```js
[
    {
        // Did you know you can do multi-value or single value docs?
        "title": "Hello, World",
        "description": "Welcome to the next generation system."
    },
    {
        "title": ["Hello, World 2 "],
        "description": ["Welcome to the next generation system. Version 2"]
    }
]
```


## Deleting Documents
Docs can only be deleted via terms, it's up to you to make sure a given term is
unique otherwise multiple docs can be deleted via this method.

NOTE: This only works with fast fields, so it's a good idea to make a unique
id or use the document_id via the `_id` field.

```
DELETE /indexes/:index_name/documents
```

### JSON Payload
```json5 
{
    // Delete any document with the value 123454 for the `id` field.
    // Make sure the type of the field aligns with the specified value type. 
    "id": 123454,
}
```

## Deleting All Documents
All docs can be cleared from the index via
```
DELETE /indexes/:index_name/documents/clear
```

## Getting A Specific Document
Getting a specific document is generally not advised as it requires making sure
the data segment is correct, so dont rely on a doc's reference address being the
same over time. 

```
GET /indexes/:index_name/documents/:document_id
```