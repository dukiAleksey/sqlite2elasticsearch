# ElasticSearch

- Understand existing SQLite data chema and create relations
- Install ES
- Create migration script from sqlite DB to ElasticSearch
- Create tests in Postman to make sure data has been migrated properly

## Installation

### Install ElasticSearch

```bash
docker run -p 9200:9200 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.7.0
```

Check status `curl http://127.0.0.1:9200`

### install python

```bash
virtualenv env
source env/bin/activate
```

## Create ES index

<details>
  <summary>Creating index code</summary>
  
    ```bash
    curl -XPUT http://127.0.0.1:9200/movies -H 'Content-Type: application/json' -d'
    {
    "settings": {
        "refresh_interval": "1s",
        "analysis": {
        "filter": {
            "english_stop": {
            "type":       "stop",
            "stopwords":  "_english_"
            },
            "english_stemmer": {
            "type": "stemmer",
            "language": "english"
            },
            "english_possessive_stemmer": {
            "type": "stemmer",
            "language": "possessive_english"
            },
            "russian_stop": {
            "type":       "stop",
            "stopwords":  "_russian_"
            },
            "russian_stemmer": {
            "type": "stemmer",
            "language": "russian"
            }
        },
        "analyzer": {
            "ru_en": {
            "tokenizer": "standard",
            "filter": [
                "lowercase",
                "english_stop",
                "english_stemmer",
                "english_possessive_stemmer",
                "russian_stop",
                "russian_stemmer"
            ]
            }
        }
        }
    },
    "mappings": {
        "dynamic": "strict",
        "properties": {
        "id": {
            "type": "keyword"
        },
        "imdb_rating": {
            "type": "float"
        },
        "genre": {
            "type": "keyword"
        },
        "title": {
            "type": "text",
            "analyzer": "ru_en",
            "fields": {
            "raw": { 
                "type":  "keyword"
            }
            }
        },
        "description": {
            "type": "text",
            "analyzer": "ru_en"
        },
        "director": {
            "type": "text",
            "analyzer": "ru_en"
        },
        "actors_names": {
            "type": "text",
            "analyzer": "ru_en"
        },
        "writers_names": {
            "type": "text",
            "analyzer": "ru_en"
        },
        "actors": {
            "type": "nested",
            "dynamic": "strict",
            "properties": {
            "id": {
                "type": "keyword"
            },
            "name": {
                "type": "text",
                "analyzer": "ru_en"
            }
            }
        },
        "writers": {
            "type": "nested",
            "dynamic": "strict",
            "properties": {
            "id": {
                "type": "keyword"
            },
            "name": {
                "type": "text",
                "analyzer": "ru_en"
            }
            }
        }
        }
    }
    }'
    ```
</details>

