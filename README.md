# HSA-module10

1. Create index
```
curl -X PUT http://localhost:9200/movies \
    -H 'Content-Type: application/json' \
    -d '{
    "mappings": {
        "dynamic":"true",
        "properties": {
            "keywords": {
                "type": "text"
            },
            "plot": {
                "type": "text"
            },
            "title": {
                "type": "text",
                "fields": {
                    "suggest": {
                        "type": "completion"
                    }
                }
            }
        }
    }
}'
```

2. Upload movies
`curl --request PUT localhost:9200/_bulk -H "Content-Type: application/json" --data-binary @movies.json`

3. Search
```
curl -X POST http://localhost:9200/movies/_search \
    -H 'Content-Type: application/json' \
    -d '{
    "suggest": {
        "movie-suggest": {
            "prefix": "captin america the",
            "completion": {
                "field": "title.suggest",
                "fuzzy": {
                    "fuzziness": 1
                }
            }
        }
    }
}'
```