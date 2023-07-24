## create

1. 使用 Index API 索引文档，如果文档存在，会先删除然后再写入，即有覆盖原内容的功能。

```
PUT article/_doc/1
{
  "id": 1,
  "title": "gin的使用",
  "author": "13sai",
  "ip": "127.0.0.1",
  "summary": "Gin 是一个用 Go (Golang) 编写的 HTTP Web 框架。",
  "published_at": "2023-02-03"
}
```

```
{
  "_index" : "article",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
```

2. Create API 中使用 POST 的方式，不需要指定文档 ID， 系统自动生成。

```
POST article/_doc
{
  "id": 2,
  "title": "go的语法",
  "author": "13sai",
  "ip": "127.0.0.1",
  "summary": "数组，channel，切片",
  "published_at": "2023-02-04"
}
```

```
{
  "_index" : "article",
  "_type" : "_doc",
  "_id" : "mgVBiIkB6c1QS2IxoZo0",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
```

> 2 比 1 效率高

3. Create API 中使用 PUT 的方式创建文档，需要指定文档 ID。如果文档已经存在，则返回 http 409 错误。

```
POST article/_create/2
{
  "id": 2,
  "title": "go的语法",
  "author": "13sai",
  "ip": "127.0.0.1",
  "summary": "数组，channel，切片",
  "published_at": "2023-02-04"
}
```

```
{
  "_index" : "article",
  "_type" : "_doc",
  "_id" : "3",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
```


## get

GET article/_doc/1
```
{
  "_index" : "article",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "id" : 1,
    "title" : "gin的使用",
    "author" : "13sai",
    "ip" : "127.0.0.1",
    "summary" : "Gin 是一个用 Go (Golang) 编写的 HTTP Web 框架。",
    "published_at" : "2023-02-03"
  }
}
```

## update

```
POST article/_update/3
{
  "doc": {
    "published_at":"2023-02-05",
    "summary": "好玩的go"
  }
}
```

```
{
  "_index" : "article",
  "_type" : "_doc",
  "_id" : "3",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 7,
  "_primary_term" : 1
}
```
### update_by_query

```
POST article/_update_by_query
{
  "query": {
    "term": {
      "id": {
        "value": 2
      }
    }
  },
  "script": {
    "source": "ctx._source.title='go 的语法update'"
  }
}
```

```
{
  "took" : 72,
  "timed_out" : false,
  "total" : 3,
  "updated" : 3,
  "deleted" : 0,
  "batches" : 1,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}
```

## delete

```
DELETE article/_doc/mgVBiIkB6c1QS2IxoZo0
```