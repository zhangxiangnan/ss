### 常见dsl：common dsl
#### 查询某个索引数据
GET xx/_search

#### 条件查询

``` json
GET xx/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "colnamexx": ["xx"]
          }
        },
        {
          "range": {
            "instanceTime": {
              "gte": "2022-05-01 00:00:00",
              "lte": "2022-05-31 23:23:00"
            }
          }
        }
      ]
    }
  }
}
```

#### 条件查询&聚合取top
``` json
GET xx/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "colxx": ["xx"]
          }
        },
        {
          "range": {
            "instanceTime": {
              "gte": "2022-05-01 00:00:00",
              "lte": "2022-05-23 23:23:00"
            }
          }
        }
      ]
    }
  },
  "aggs": {
    "taskGroup": {
      "terms": {
        "field": "xx",
        "size": 100
      },
      "aggs": {
        "detailList": {
          "top_hits": {
            "size": 100
          }
        }
      }
    }
  }
}
```