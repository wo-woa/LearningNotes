## 官方文档

query_string

https://www.elastic.co/guide/en/elasticsearch/reference/7.6/query-dsl-query-string-query.html





## keyword 和 term 忽略大小写

在 [Elasticsearch ](https://coding.imooc.com/class/181.html?mc_marking=397b491f610fd9c89e0ad95aad9e9886&mc_channel=shouji)中处理字符串类型的数据时，如果我们想把整个数据作为一个完整的 term 存储，我们通常会将其类型（ `type）` 设定为 `keyword`。而这种设定又会给我们带来麻烦，比如 `Bar`、`bar`两个实际都是 `bar`，但当我们去搜索 `bar`时却无法返回 Bar的文档。要解决这个问题，就需要 `Normalizer`出场了！

```JSON
PUT test_normalizer
{
  "settings": {
    "analysis": {
      "normalizer": {
        "lowercase": {
          "type": "custom",
          "filter": ["lowercase"]
        }
      }
    }
  },
  "mappings": {
      "properties": {
        "foo": {
          "type": "keyword"
        },
        "foo_normalizer": {
          "type": "keyword",
          "normalizer": "lowercase"
        }
    }
  }
}
```

1、normalizer是 keyword的一个属性，可以对 keyword生成的单一 Term再做进一步的处理，比如 lowercase，即做小写变换。使用方法和自定义分词器有些类似。

2、我们第一步是自定义了名为 lowercase的 normalizer，其中filter 类似自定义分词器中的 filter ，normalizer中可可用的filtr种类很少，详情大家可以查看官方文档。

3、通过 normalizer属性设定到指定字段type_normalizer中；

流程说明：

1. es文档写入时由于对字段指定了 `normalizer`,那该字段的 `term`都会被做小写处理
2. 查询时搜索词同样采用有 `normalizer`的配置，因此处理后的 `term`也是小写的
3. 这样写入数据和搜索都忽略大小写，就得到了我们上面的结果



## _source、_all、store和index属性

https://www.cnblogs.com/sunfie/p/10171198.html

我们把原始文档写入Elasticsearch，默认情况下，Elasticsearch里面有2份内容，一份是原始文档，也就是_source字段里的内容，我们在Elasticsearch中搜索文档，查看的文档内容就是_source中的内容

另一份是倒排索引，倒排索引中的数据结构是倒排记录表，记录了词项和文档之间的对应关系，比如关键词”中国人”包含在文档ID为1的文档中，倒排记录表中存储的就是这种对应关系，当然也包括词频等更多信息。Elasticsearch底层用的是Lucene的API，Elasticsearch之所以能完成全文搜索的功能就是因为存储的有倒排索引。

使用normalizer修改了倒排索引，添加了大小写，使大小写都可以搜索到，显示的是source中的值

公司中删除了source，使用store存储和显示，而使用normalizer会影响到store中的值，将原有的大写都改为了小写。



## analysis

常用的属性有type，tokenizer，char_filter，filter，pattern和lowercase。

type是类型，一般有custom，whitespace和lowercase，为custom才会用到tokenizer

tokenizer为分词规则，当type为custom时使用。具体有http://doc.codingdict.com/elasticsearch/346/，如keyword就是不分词。

char_filter没有具体用过，可以参考https://www.cnblogs.com/cjsblog/p/10171695.html

filter说是过滤器，但是实际上类似于转换器，常用的有lowercase和asciifolding，将分词之后的单词的所有大小写形式放入倒排索引中,更多参考http://doc.codingdict.com/elasticsearch/381/

pattern就是使用正则进行分词，当type为pattern时使用，常用的有[,:;/()]





## 聚合 aggs

https://blog.csdn.net/weixin_40341116/article/details/81173016

### 聚合概念

- 聚合允许我们向数据提出一些複杂的问题，虽然他的功能完全不同于搜索，但他们其实使用了相同的数据结构，这表示聚合的执行速度很快，并且就像搜索一样几乎是实时的
  - 并且由于聚合和搜索是使用同样的数据结构，因此聚合和搜索可以是一起执行的
  - 这表示我们可以在一次json请求裡，同时对相同的数据进行 搜索/过滤 + 分析，两个愿望一次满足
- 聚合的两个主要的概念，分别是 ==桶 和 指标==
  - 桶(Buckets) : 满足特定条件的文档的集合
    - 当聚合开始被执行，每个文档会决定符合哪个桶的条件，如果匹配到，文档将放入相应的桶并接着进行聚合操作
      - 像是一个员工属于男性桶或者女性桶，日期2014-10-28属于十月桶，也属于2014年桶
    - 桶可以被嵌套在其他桶里面
      - 像是北京能放在中国桶裡，而中国桶能放在亚洲桶裡
    - Elasticsearch提供了很多种类型的桶，像是时间、最受欢迎的词、年龄区间、地理位置桶等等，不过他们在根本上都是通过同样的原理进行操作，也就是基于条件来划分文档，一个文档只要符合条件，就可以加入那个桶，因此一个文档可以同时加入很多桶
  - 指标(Metrics) : 对桶内的文档进行统计计算
    - 桶能让我们划分文档到有意义的集合， 但是最终我们需要的是对这些桶内的文档进行一些指标的计算
    - 指标通常是简单的数学运算(像是min、max、avg、sum），而这些是通过当前桶中的文档的值来计算的，利用指标能让你计算像平均薪资、最高出售价格、95%的查询延迟这样的数据
- aggs 聚合的模板
  - 当query和aggs一起存在时，会先执行query的主查询，主查询query执行完后会搜出一批结果，而这些结果才会被拿去aggs拿去做聚合
    - 另外要注意aggs后面会先接一层自定义的这个聚合的名字，然后才是接上要使用的聚合桶
    - 如果有些情况不在意查询结果是什麽，而只在意aggs的结果，可以把size设为0，如此可以让返回的hits结果集是0，加快返回的速度
  - 一个aggs裡可以有很多个聚合，每个聚合彼此间都是独立的，因此可以一个聚合拿来统计数量、一个聚合拿来分析数据、一个聚合拿来计算标准差...，让一次搜索就可以把想要做的事情一次做完
    - 像是此例就定义了3个聚合，分别是custom_name1、custom_name2、custom_name3
  - aggs可以嵌套在其他的aggs裡面，而嵌套的桶能作用的文档集范围，是外层的桶所输出的结果集

```java
GET 127.0.0.1/mytest/doc/_search
{
    "query": { ... },
    "size": 0,
    "aggs": {
        "custom_name1": {  //aggs后面接著的是一个自定义的name
            "桶": { ... }  //再来才是接桶
        },
        "custom_name2": {  //一个aggs裡可以有很多聚合
            "桶": { ... }
        },
        "custom_name3": {
            "桶": {
               .....
            },
            "aggs": {  //aggs可以嵌套在别的aggs裡面
                "in_name": { //记得使用aggs需要先自定义一个name
                    "桶": { ... } //in_name的桶作用的文档是custom_name3的桶的结果
                }
            }
        }
    }
}
```

结果

```java
  {
   "hits": {
       "total": 8,
       "max_score": 0,
       "hits": [] //因为size设为0，所以没有查询结果返回
   },
   "aggregations": {
       "custom_name1": {
           ...
       },
       "custom_name2": {
           ...
       },
       "custom_name3": {
           ... ,
           "in_name": {
              ....
           }
       }
   }
  }
```



### 聚合中常用的桶terms、filter、top_hits

#### terms桶 

针对某个field的值进行分组，field有几种值就分成几组

- terms桶在进行分组时，会爲此field中的每种值创建一个新的桶

- 要注意此 "terms桶" 和平常用在主查询query中的 "查找terms" 是不同的东西

- 具体实例

  - 首先插入几笔数据，其中color是一个keyword类型

  ```java
    { "color": "red" }
    { "color": "green" }
    { "color": ["red", "blue"] }
  ```

  - 执行terms聚合

  ```java
    GET 127.0.0.1/mytest/doc/_search
    {
        "query": {
            "match_all": {}
        },
        "size": 0,
        "aggs": {
            "my_name": {
                "terms": {
                    "field": "color" //使用color来进行分组
                }
            }
        }
    }
  ```

  - 结果

    - 因为color总共有3种值，red、blue、green，所以terms桶为他们产生了3个bucket，并计算了每个bucket中符合的文档有哪些
    - bucket和bucket间是独立的，也就是说一个文档可以同时符合好几个bucket，像是`{"color": ["red", "blue"]}`就同时符合了red和blue bucket

  ```java
    "aggregations": {
        "my_name": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [
                {
                    "key": "blue",
                    "doc_count": 1
                },
                {
                    "key": "red",
                    "doc_count": 2  //表示color为red的文档有2个，此例中就是 {"color": "red"} 和 {"color": ["red", "blue"]}这两个文档
                },
                {
                    "key": "green",
                    "doc_count": 1
                }
            ]
        }
    }
  ```

- 具体实例二

  - 将terms桶搭配度量指标(avg、min、max、sum...)一起使用

    - 其实度量指标也可以看成一种"桶"，他可以和其他正常的桶们进行嵌套作用，差别只在指标关注的是这些文档中的某个数值的统计，而桶关注的是文档

  - 首先准备数据，color一样为keyword类型，而price为integer类型

   ```java
    { "color": "red", "price": 100 }
    { "color": "green", "price": 500 }
    { "color": ["red", "blue"], "price": 1000 }
   ```

  - 将avg指标嵌套在terms桶裡一起使用

   ```java
    GET 127.0.0.1/mytest/doc/_search
    {
        "query": {
            "match_all": {}
        },
        "size": 0,
        "aggs": {
            "my_name": {
                "terms": {
                    "field": "color"
                },
                "aggs": {  //嵌套两个指标avg、min在terms桶中
                    "my_avg_price": { //my_avg_price计算每个bucket的平均price
                        "avg": {
                            "field": "price"
                        }
                    },
                    "my_min_price": { //my_min_price计算每个bucket中的最小price
                        "min": {
                            "field": "price"
                        }
                    }
                }
            }
        }
    }
   ```

  - 结果

   ```java
    "aggregations": {
        "my_name": {
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0,
            "buckets": [ //terms桶中的每个bucket都会计算avg和min两个指标
                {
                    "key": "blue",
                    "doc_count": 1,
                    "my_avg_price": { //avg指标
                        "value": 1000
                    },
                    "my_min_price": { //min指标
                        "value": 100
                    }
                },
                {
                    "key": "red",
                    "doc_count": 2,
                    "my_avg_price": {  //avg指标计算的值，因为符合color为red的文档有两笔，所以平均price为100+1000/2 = 550
                        "value": 550
                    },
                    "my_min_price": {
                        "value": 100
                    }
                },
                {
                    "key": "green",
                    "doc_count": 1,
                    "my_avg_price": {
                        "value": 500
                    },
                    "my_min_price": {
                        "value": 500
                    }
                }
            ]
        }
    }
   ```

#### filter桶 

一个用来过滤的桶

  - 要注意此处的 "filter桶" 和用在主查询query的 "过滤filter" 的用法是一模一样的，都是过滤

  - 不过差别是 "filter桶" 会自己给创建一个新的桶，而不会像 "过滤filter" 一样依附在query下

    - 因为filter桶毕竟还是一个聚合桶，因此他可以和别的桶进行嵌套，但他不是依附在别的桶上

  - 具体实例

    - 取得color为red或是blue的文档

     ```java
      GET 127.0.0.1/mytest/doc/_search
      {
          "query": {
              "match_all": {}
          },
          "size": 0,
          "aggs": {
              "my_name": {
                  "filter": { //因为他用法跟一般的过滤filter一样，所以也能使用bool嵌套
                      "bool": {
                          "must": {
                              "terms": { //注意此terms是查找terms，不是terms桶
                                  "color": [ "red", "blue" ]
                              }
                          }
                      }
                  }
              }
          }
      }
     ```

    - 结果

     ```java
      "aggregations": {
          "my_name": {
              "doc_count": 2 //filter桶计算出来的文档数量
          }
      }
     ```

  - 具体实例二

    - filter桶和terms桶嵌套使用，先过滤出color为red以及blue的文档，再对这些文档进行color分组

     ```java
      GET 127.0.0.1/mytest/doc/_search
      {
          "query": {
              "match_all": {}
          },
          "size": 0,
          "aggs": {
              "my_name": { //my_name聚合
                  "filter": { //filter桶
                      "bool": {
                          "must": {
                              "terms": {
                                  "color": [ "red", "blue" ]
                              }
                          }
                      }
                  },
                  "aggs": {
                      "my_name2": { //my_name2聚合，嵌套在my_name聚合裡
                          "terms": { //terms桶
                              "field": "color"
                          }
                      }
                  }
              }
          }
      }
     ```

    - 结果

      - 因为terms桶嵌套在filter桶内，所以query查询出来的文档们会先经过filter桶，如果符合filter桶，才会进入到terms桶内
      - 此处通过filter桶的文档只有两笔，分别是`{"color": "red"}`以及`{"color": ["red", "blue"]}`，所以terms桶只会对这两笔文档做分组
      - 这也是为什麽terms桶裡没有出现color为green的分组，因为这个文档在filter桶就被挡下来了

     ```java
      "aggregations": {
          "my_name": {
              "doc_count": 2, //filter桶计算的数量，通过此处的文档只有2笔
              "my_name2": {
                  "doc_count_error_upper_bound": 0,
                  "sum_other_doc_count": 0,
                  "buckets": [ 
                      {
                          "key": "red",
                          "doc_count": 2  //terms桶计算的数量
                      },
                      {
                          "key": "blue",
                          "doc_count": 1  //terms桶计算的数量
                      }
                  ]
              }
          }
      }
     ```

#### top_hits桶
在某个桶底下找出这个桶的前几笔hits，返回的hits格式和主查询query返回的hits格式一模一样

  - top_hits桶支持的参数

    - `from`、`size`
    - `sort` : 设置返回的hits的排序
      - 要注意，假设在主查询query裡已经对数据设置了排序`sort`，此`sort`并不会对aggs裡面的数据造成影响，也就是说主查询query查找出来的数据会先丢进aggs而非先经过`sort`，因此就算主查询设置了`sort`，也不会影响aggs数据裡的排序
      - 因此如果在top_hits桶裡的返回的hits数据想要排序，需要自己在top_hits桶裡设置`sort`
      - 如果没有设置sort，默认使用主查询query所查出来的`_score`排序
    - `_source` : 设置返回的字段

  - 具体实例

    - 首先准备数据，color是keyword类型

      ```java
      { "color": "red", "price": 100 }
      { "color": ["red", "blue"], "price": 1000 }
      ```

    - 使用terms桶分组，再使用top_hits桶找出每个group裡面的price最小的前5笔hits

      ```java
      GET 127.0.0.1/mytest/doc/_search
      {
          "query": {
              "match_all": {}
          },
          "size": 0,
          "aggs": {
              "my_name": {
                  "terms": {
                      "field": "color"
                  },
                  "aggs": {
                      "my_top_hits": {
                          "top_hits": {
                              "size": 5,
                              "sort": {
                                  "price": "asc"
                              }
                          }
                      }
                  }
              }
          }
      }
      ```

    - 结果

      ```java
      "aggregations": {
          "my_name": {
              "doc_count_error_upper_bound": 0,
              "sum_other_doc_count": 0,
              "buckets": [
                  {
                      "key": "red",
                      "doc_count": 2,  //terms桶计算出来的color为red的文档数
                      "my_top_hits": {
                          "hits": {  //top_hits桶找出color为red的这些文档中，price从小到大排序取前5笔
                              "total": 2,
                              "max_score": null,
                              "hits": [
                                  {
                                      "_score": null,
                                      "_source": { "color": "red", "price": 100 },
                                      "sort": [ 100 ]
                                  },
                                  {
                                      "_score": null,
                                      "_source": { "color": [ "red", "blue" ], "price": 1000 },
                                      "sort": [ 1000 ]
                                  }
                              ]
                          }
                      }
                  },
                  {
                      "key": "blue",
                      "doc_count": 1,  //terms桶计算出来的color为blue的文档数
                      "my_top_hits": {
                          "hits": { //top_hits桶找出的hits
                              "total": 1,
                              "max_score": null,
                              "hits": [
                                  {
                                      "_source": {
                                          "color": [ "red", "blue" ], "price": 1000 },
                                      "sort": [ 1000 ]
                                  }
                              ]
                          }
                      }
                  }
              ]
          }
      }
      ```

