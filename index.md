### create
```
# input
PUT article
{
  "mappings": {
    "properties": {
        "id": {
          "type": "integer"
        },
        "title": {
          "type": "text"
        },
        "author": {
          "type": "keyword"
        },
        "ip": {
          "type": "ip"
        },
        "summary": {
          "type": "text"
        },
        "published_at": {
          "type": "date"
        }
      }
  },
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 1
  }
}
```

## delete

```
DELETE article
```

```
{
  "acknowledged" : true
}
```