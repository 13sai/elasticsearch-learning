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

## alias 

### add alias

```
POST /_aliases
{
  "actions" : [
    { "add" : { "index" : "article", "alias" : "article_alias1" } }
  ]
}
```

还可以加一批，支持 [date math](https://www.elastic.co/guide/en/elasticsearch/reference/7.13/date-math-index-names.html).

```
POST /_aliases
{
  "actions" : [
    { "add" : { "index" : "logs", "alias" : "<logs_{now/M}>" } }
  ]
}
```

多个
```
POST /_aliases
{
  "actions" : [
    { "add" : { "index" : "test1", "alias" : "alias1" } },
    { "add" : { "index" : "test2", "alias" : "alias1" } }
  ]
}

<!-- 注意这里是 indices -->
POST /_aliases
{
  "actions" : [
    { "add" : { "indices" : ["test1", "test2"], "alias" : "alias1" } }
  ]
}
```

### remove alias 

```
POST /_aliases
{
  "actions" : [
    { "add" : { "remove" : "article", "alias" : "article_alias1" } }
  ]
}
```

### rename alis

```
POST /_aliases
{
  "actions" : [
    { "remove" : { "index" : "article", "alias" : "article_alias1" } },
    { "add" : { "index" : "article", "alias" : "article_alias2" } }
  ]
}
```

