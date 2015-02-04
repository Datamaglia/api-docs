# Recommendations API
Note that the `Auth-Token` headers for the public API are application tokens,
not authentication tokens (which the developers can't access since they are only
used by the private API).

# /recommendations/{app}/subgraphs/{subgraph}/member{?limit,id}

## GET
Retrieve a recommendation for a current member of the subgraph.

+ Parameters
    + app (required, string, `f321f321f321f321`) ... ID of the application from
      which to generate a recommendation.
    + subgraph (required, string, `a42ea42ea42ea42e`) ... ID of the recommender
      subgraph from which to generate recommendations.
    + limit = `5` (optional, number, `2`) ... The maximum number of
      recommendations to return.
    + id (required, string, `janedoe@example.com`) ... User identifier
      associated with the user for whom recommendations are being generated.

+ Request JSON

    + Headers

            Accept: application/json
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

+ Response 200 (application/json)

    + Body

            {
                "entities": [
                    {
                       "score": 0.9778
                        "relationship": {
                            "type": "Likes",
                            "weight": 4.0,
                            "source": {
                              "id": {
                                "key": "email",
                                "value": "bob@loblaw.com"
                              } 
                            },
                            "target": {
                                "id": {
                                    "key": "imdb",
                                    "value": "tt0398286"
                                },
                                "properties": [
                                  {
                                    "key": "title",
                                    "value": "Tangled"
                                  }
                                ]
                            }
                        }
                    },
                    ...
                ]
            }

# /applications/{app}/subgraphs/{subgraph}/data/sources/

+ Parameters
    + app (required, string, `f321f321f321f321`) ... ID of the application from
      which to generate a recommendation.
    + subgraph (required, string, `a42ea42ea42ea42e`) ... ID of the recommender
      subgraph from which to generate recommendations.

## POST
Insert source entities into the subgraph. If entities exist, they will be
updated. For bulk data loading, use the CSV endpoint.

+ Request JSON

    + Headers

            Content-Type: application/json
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {
                "entities": [
                    {
                        "id": {
                            "key": "email",
                            "value": "johndoe@example.com"
                        },
                        "properties": [
                            {
                                "key": "name",
                                "value": "John Doe"
                            }
                        ]
                    },
                    ...
                ]
            }

+ Response 204

+ Request Line-delimited JSON

    + Headers

            Content-Type: application/x-ldjson
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {"id": {"email": "johndoe@example.com"}, "properties": { "name": "John Doe"}}
            {"id": {"email": "janedoe@example.com"}, "properties": { "name": "Jane Doe"}}

+ Response 204

# /applications/{app}/subgraphs/{subgraph}/data/targets/

+ Parameters
    + app (required, string, `f321f321f321f321`) ... ID of the application from
      which to generate a recommendation.
    + subgraph (required, string, `a42ea42ea42ea42e`) ... ID of the recommender
      subgraph from which to generate recommendations.

## POST
Insert target entities into the subgraph. If entities already exist, they will
be updated. For bulk data loading, use the CSV endpoint.

+ Request JSON

    + Headers

            Content-Type: application/json
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {
                "entities": [
                    {
                        "id": {
                           "key": "imdb",
                            "value": "tt0398286"
                        },
                        "properties": [ 
                            {
                                "key": "title",
                                "value": "Tangled"
                            }
                        ]
                    },
                    ...
                ]
            }

+ Response 204

+ Request Line-delimited JSON

    + Headers

            Content-Type: application/x-ldjson
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {"id": {"imdb": "tt0398286"}, "properties": {"title": "Tangled"}}
            {"id": {"imdb": "tt1217209"}, "properties": {"title": "Brave"}}

+ Response 204

# /applications/{app}/subgraphs/{subgraph}/data/relationships/

+ Parameters
    + app (required, string, `f321f321f321f321`) ... ID of the application from
      which to generate a recommendation.
    + subgraph (required, string, `a42ea42ea42ea42e`) ... ID of the recommender
      subgraph from which to generate recommendations.

## POST
Create relationships between sources and targets in the subgraph. For bulk data
loading, use the CSV endpoint.

Note that the source and target properties of the JSON request body have the
same structure used by the source and target insertion endpoints. This means
that you can create sources and targets with the relationships endpoint by
simply specifying entities that don't yet exist. Properties will be created as
well.

+ Request JSON

    + Headers

            Content-Type: application/json
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {
                "entities": [
                    {
                        "type": "Likes",
                        "weight": 4.0,
                        "source": {
                            "id": {
                                "key": "email",
                                "value": "johndoe@example.com"
                            },
                            "properties": [
                                 {
                                    "key": "name",
                                    "value": "John Doe"
                                 }
                            ]
                        },
                        "target": {
                            "id": {
                                "key": "imdb",
                                "value": "tt0398286"
                            },
                            "properties": [
                                "key": "title",
                                "value": "Tangled"
                            ]
                        }
                    },
                    ...
                ]
            }

+ Response 204

+ Request Line-delimited JSON

    + Headers

            Content-Type: application/x-ldjson
            Auth-Token: d44ed44ed44ed44ed44ed44ed44ed44e

    + Body

            {"type": "Likes", "source": {"id": {"email": "johndoe@example.com"}}, "target": {"id": {"imdb": "tt0398286"}}}
            {"type": "Likes", "source": {"id": {"email": "johndoe@example.com"}}, "target": {"id": {"imdb": "tt1217209"}}}

+ Response 204
