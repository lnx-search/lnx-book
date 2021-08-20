# Searching An index
Once you've added documents to you're ready to start
searching!

lnx provides you with 3 major ways to query the index:
- (`mode=normal`) The tantivy query parse system, this is not typo tolerant
but is very powerful for custom user queries, think log searches.
- (`mode=fuzzy`, DEFAULT) A fuzzy query, this ignores the custom query 
system that the standard query parser would otherwise handle,
but intern is typo tolerant.
- (`mode=more-like-this`) Unlike the previous two options this takes a document
reference address and produces documents similar to the given one.
This is super useful for things like books etc... Wanting related items.

### Required Endpoints
```
GET /indexes/:index_name/search
```

**Endpoint Query Parameters**
- `query`* - A string query which is used to search documents.
- `document`** - A document id in the form of a integer.
- `mode` - The specified mode of the query. Out of `normal`, `fuzzy`, `more-like-this`.
- `limit` - Limit the amount of results to return (Default: `20`)
- `offset` - The offset to skip `n` documents before returning any. 
- `order_by` - The field to order content by. This must be a field existing in 
the schema.

`*` = This is required for `normal` and `fuzzy` mode queries but not `more-like-this`.</br>
`**` = This is required for `more-like-this` queries but not `normal` and `fuzzy`.

### Example Search Query
The bellow query searches out `movies` index with the query `hello, world`
this will be matched in the `fuzzy` mode due to it being the default if not
specified, we then limit it to `10` items.
```
GET /indexes/movies/search?limit=10&query=hello, world
```

Our response can look something like this:
```js
{
    "data": {
        "count": 4,
        "hits": [
            {
                "doc": {
                    "genres": [
                        "Action",
                        "Comedy",
                        "Fantasy"
                    ],
                    "id": [
                        "287947"
                    ],
                    "overview": [
                        "A boy is given the ability to become an adult superhero in times of need with a single magic word."
                    ],
                    "poster": [
                        "https://image.tmdb.org/t/p/w500/xnopI5Xtky18MPhK40cZAGAOVeV.jpg"
                    ],
                    "release_date": [
                        1553299200
                    ],
                    "title": [
                        "Shazam!"
                    ]
                },
                "document_id": "4354052675788172051"
            },
            {
                "doc": {
                    "genres": [
                        "Animation",
                        "Family",
                        "Adventure"
                    ],
                    "id": [
                        "166428"
                    ],
                    "overview": [
                        "As Hiccup fulfills his dream of creating a peaceful dragon utopia, Toothless’ discovery of an untamed, elusive mate draws the Night Fury away. When danger mounts at home and Hiccup’s reign as village chief is tested, both dragon and rider must make impossible decisions to save their kind."
                    ],
                    "poster": [
                        "https://image.tmdb.org/t/p/w500/xvx4Yhf0DVH8G4LzNISpMfFBDy2.jpg"
                    ],
                    "release_date": [
                        1546473600
                    ],
                    "title": [
                        "How to Train Your Dragon: The Hidden World"
                    ]
                },
                "document_id": "12457370295431745695"
            },
            {
                "doc": {
                    "genres": [
                        "Action",
                        "Adventure",
                        "Science Fiction"
                    ],
                    "id": [
                        "299537"
                    ],
                    "overview": [
                        "The story follows Carol Danvers as she becomes one of the universe’s most powerful heroes when Earth is caught in the middle of a galactic war between two alien races. Set in the 1990s, Captain Marvel is an all-new adventure from a previously unseen period in the history of the Marvel Cinematic Universe."
                    ],
                    "poster": [
                        "https://image.tmdb.org/t/p/w500/AtsgWhDnHTq68L0lLsUrCnM7TjG.jpg"
                    ],
                    "release_date": [
                        1551830400
                    ],
                    "title": [
                        "Captain Marvel"
                    ]
                },
                "document_id": "14969987396162595734"
            },
            {
                "doc": {
                    "genres": [
                        "Thriller",
                        "Action",
                        "Horror",
                        "Science Fiction"
                    ],
                    "id": [
                        "522681"
                    ],
                    "overview": [
                        "Six strangers find themselves in circumstances beyond their control, and must use their wits to survive."
                    ],
                    "poster": [
                        "https://image.tmdb.org/t/p/w500/8Ls1tZ6qjGzfGHjBB7ihOnf7f0b.jpg"
                    ],
                    "release_date": [
                        1546473600
                    ],
                    "title": [
                        "Escape Room"
                    ]
                },
                "document_id": "14536447352761414553"
            }
        ],
        "time_taken": 0.0005308
    },
    "status": 200
}
```