# 1ST QUERY: [header: x-hasura-admin-secret]
query getTracks($genre: String, $limit: Int, $offset: Int) {
track (limit: $limit, offset: $offset, where: {genre: {name: {_eq: $genre}}})
{
name
genre_id
}
}

- Query Variable:
{
"genre":"Metal",
"limit": 5,
"offset":50
}

- OUTPUT:
{
  "data": {
    "track": [
      {
        "name": "Trupets Of Jericho",
        "genre_id": 3
      },
      {
        "name": "Machine Men",
        "genre_id": 3
      },
      {
        "name": "The Alchemist",
        "genre_id": 3
      },
      {
        "name": "Realword",
        "genre_id": 3
      },
      {
        "name": "Free Speech For The Dumb",
        "genre_id": 3
      }
    ]
  }
}


# 2ND QUERY: [Header: x-hasura-artist-id = 5]
query getAlbumsAsArtist{
album {
title
}
}

- OUTPUT:
{
  "data": {
    "album": [
      {
        "title": "Facelift"
      }
    ]
  }
}


# 3RD QUERY:
query trackValue {
track_aggregate {
aggregate {
sum {
unit_price
}
}
}
}

- OUTPUT:
	- With header: x-hasura-admin-secret
{
  "data": {
    "track_aggregate": {
      "aggregate": {
        "sum": {
          "unit_price": 3680.97
        }
      }
    }
  }
}

	- With header: x-hasura-artist-id = 5
{
  "errors": [
    {
      "message": "field 'track_aggregate' not found in type: 'query_root'",
      "extensions": {
        "path": "$.selectionSet.track_aggregate",
        "code": "validation-failed"
      }
    }
  ]
}

# COMPLEX QUERY
```
query getTrackDetails($limit: Int, $offset: Int) {
  track(limit: $limit, offset: $offset) {
    track_id
    name
    unit_price
    album {
      album_id
      title
      artist {
        artist_id
        name
      }
    }
    genre {
      genre_id
      name
    }
  }
  track_aggregate {
    aggregate {
      sum {
        unit_price
      }
    }
  }
}
```

- QUERY VARIABLES
```
{
  "limit": 10,
  "offset": 0
}
```

- OUTPUT
```
{
  "data": {
    "track": [
      {
        "track_id": 1,
        "name": "For Those About To Rock (We Salute You)",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 2,
        "name": "Balls to the Wall",
        "unit_price": 0.99,
        "album": {
          "album_id": 2,
          "title": "Balls to the Wall",
          "artist": {
            "artist_id": 2,
            "name": "Accept"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 3,
        "name": "Fast As a Shark",
        "unit_price": 0.99,
        "album": {
          "album_id": 3,
          "title": "Restless and Wild",
          "artist": {
            "artist_id": 2,
            "name": "Accept"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 4,
        "name": "Restless and Wild",
        "unit_price": 0.99,
        "album": {
          "album_id": 3,
          "title": "Restless and Wild",
          "artist": {
            "artist_id": 2,
            "name": "Accept"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 5,
        "name": "Princess of the Dawn",
        "unit_price": 0.99,
        "album": {
          "album_id": 3,
          "title": "Restless and Wild",
          "artist": {
            "artist_id": 2,
            "name": "Accept"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 6,
        "name": "Put The Finger On You",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 7,
        "name": "Let's Get It Up",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 8,
        "name": "Inject The Venom",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 9,
        "name": "Snowballed",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      },
      {
        "track_id": 10,
        "name": "Evil Walks",
        "unit_price": 0.99,
        "album": {
          "album_id": 1,
          "title": "For Those About To Rock We Salute You",
          "artist": {
            "artist_id": 1,
            "name": "AC/DC"
          }
        },
        "genre": {
          "genre_id": 1,
          "name": "Rock"
        }
      }
    ],
    "track_aggregate": {
      "aggregate": {
        "sum": {
          "unit_price": 3680.97
        }
      }
    }
  }
}
```

- RESPONSE TIME WITH CACHING:
Response Time
76 ms
Response Size
839 bytes


- RESPONSE TIME WITHOUT CACHING:
Response Time
95 ms
Response Size
839 bytes

To Disable Caching in Docker Compose, update the docker-compose.yaml file:
```
    # Remove or comment out caching-related environment variables
    # HASURA_GRAPHQL_REDIS_URL: "redis://redis:6379"
    # HASURA_GRAPHQL_RATE_LIMIT_REDIS_URL: "redis://redis:6379"
```
By commenting out the HASURA_GRAPHQL_REDIS_URL and HASURA_GRAPHQL_RATE_LIMIT_REDIS_URL environment variables, we ensure that Redis caching is not used and Hasura will then operate without caching.