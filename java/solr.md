# 查询方式

https://blog.csdn.net/magicpenta/article/details/82354106

## URL 查询

实际上，Solr 管理页面是通过 URL 的方式请求数据的。例如上例的查询，转成相应的 URL 就是 `http://localhost:8983/solr/testCollection/select?q=*:*`。

在这个 URL 中，`testCollection` 表示集合名，`select` 表示查询动作，参数 `q` 表示查询条件。其中，参数 `q` 最为关键，它决定了哪些数据可以返回给用户。

现在，我们在浏览器地址栏中输入这个 URL，即可得到 `json` 格式的数据。



## 查询参数

除了上述提到的查询条件 `q`，Solr 还有其他查询参数，具体见下表：

| 参数名 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| q      | 查询条件，必填项                                             |
| start  | 结果集第一条记录的偏移位置，用于分页，默认为 0               |
| rows   | 返回文档的记录数，用于分页，默认为 10                        |
| sort   | 排序                                                         |
| fl     | 指定返回字段，多个字段用逗号或空格分隔，默认返回所有字段     |
| wt     | 指定输出格式，例如xml、json等                                |
| fq     | 过滤查询。该参数可将查询的结果限定在某一范围，由于 Solr 会对过滤查询进行缓存，因此它可以显著提升复杂查询的效率 |
| hl     | 用于设置字段的高亮显示                                       |



## 运算符

与 SQL 相似，Solr 查询语句中也包含运算符，具体见下表：

| 运算符 | 说明                                        |
| ------ | ------------------------------------------- |
| :      | 指定字段目标值，等同于 SQL 中的 “=” 号      |
| ?      | 通配符，替代任意单个字符                    |
| *      | 通配符，替代任意多个字符                    |
| AND    | 表示且，等同于 “&&”                         |
| OR     | 表示或，等同于 “\|\|”                       |
| NOT    | 表示否                                      |
| ()     | 用于构成子查询                              |
| []     | 范围查询，包含头尾                          |
| {}     | 范围查询，不包含头尾                        |
| +      | 存在运算符，表示文档中必须存在 “+” 号后的项 |
| -      | 不存在运算符，表示文档中不包含 “-” 号后的项 |

# Schema介绍

https://blog.csdn.net/supermao1013/article/details/83628344

Schema：模式，是集合/内核中字段的定义，让solr知道集合/内核包含哪些字段、字段的数据类型、字段该索引存储。

------

Solr中提供了两种方式来配置schema，两者只能选其一

> - 默认方式，通过Schema API 来实时配置，模式信息存储在内核目录的conf/managed-schema.xml文件中，一般使用这种方式，特别是在SolrCloud模式下。
> - 传统的手工编辑内核目录的conf/schema.xml的方式，编辑完后需重载集合/内核才会生效。

两种方式之间是可以切换的，比如用于升级操作，从旧版本到新版本的升级，切换方式如下：

> - **手动编辑切换到API方式：** 只需将solrconfig.xml中的 去掉，或改为ManagedIndexSchemaFactory。Solr重启时，它发现之前存储在schema.xml中但没有存储在 managed-schema.xml中，则它会备份schema.xml，然后改写schema.xml为managed-schema.xml。此后就可以通过Schema API 管理schema了
> - **API切换到手动编辑方式：** 首先将managed-schema.xm重命名为schema.xml；然后将solrconfig.xml中schemaFactory 的ManagedIndexSchemaFactory去掉（如果存在）；最后增加

除此之外，Solr还支持无模式的方式，这种情况Solr会猜测该如何索引字段，这种方式不能应用在生产环境，也比较少用。

## managed-schema.xml文件详解

Solr启动一个服务器实例后，会内置了很多field字段、唯一ID、fieldType字段类型等，这些信息都是在managed-schema.xml文件中定义的。我们先了解下managed-schema.xml文件的大致结构：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<schema version="1.6">
	<field .../>  
	<dynamicField .../>
	<uniqueKey>id</uniqueKey>
	<copyField .../>
	<fieldType ...>
		<analyzer type="index">
			<tokenizer .../>
			<filter ... />
		</analyzer>
		<analyzer type="query">
			<tokenizer.../>
			<filter ... />
		</analyzer>
	</fieldType>
</schema>
```

### Field详解

```xml
<field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" />
<field name="_version_" type="plong" indexed="false" stored="false"/>
<field name="_root_" type="string" indexed="true" stored="false" docValues="false" />
<field name="_text_" type="text_general" indexed="true" stored="false" multiValued="true"/>
```

字段属性说明：

> - name：字段名，必需。字段名可以由字母、数字、下划线构成，不能以数字开头。以下划线开头和结尾的名字为保留字段名，如 *version*
> - type：字段的fieldType名，必需。为 FieldType标签中定义的name属性值。
> - default：默认值，非必须。如果提交的文档中没有该字段的值，则自动会为文档添加这个默认值。相当于传统数据库中的字段默认值。

除此之外，字段还有很多可选属性如下：

| 属性名称                                           | 描述                                                         | 可选值      | 默认值 |
| -------------------------------------------------- | ------------------------------------------------------------ | ----------- | ------ |
| indexed                                            | 是否索引                                                     | true、false | true   |
| stored                                             | 是否存储                                                     | true、false | true   |
| docValues                                          | 是否为该字段创建字段的列式存储（docValues)                   | true、false | false  |
| sortMissingFirst sortMissingLast                   | 根据该字段排序时，没有该字段值的文档是排在前面还是后面       | true、false | false  |
| multiValued                                        | 字段是否是多值的                                             | true、false | false  |
| omitNorms                                          | 是否忽略标准化。对于所有 primitive (non-analyzed) field types, such as int, float, data, bool, and string，默认是true. Only full-text fields or fields need norms | true、false | *      |
| omitTermFreqAndPositions                           | 是否忽略词项的词频和位置                                     | true、false | *      |
| omitPositions                                      | 忽略词项的位置信息                                           | true、false | *      |
| termVectors termPositions termOffsets termPayloads | 词项向量的存储                                               | true、false | false  |
| required                                           | 字段是否必需                                                 | true、false | false  |
| useDocValuesAsStored                               | 如果字段设置了存储docValues，而字段的stored=false，可以设置该属性为true，从而可以在搜索结果中返回该字段的值（从docValues中取值） | true、false | true   |
| large                                              | 标识该字段的值是大尺寸的，从而对该字段值进行懒加载，只有值 < 512KB 的才会被缓存。 这个属性要求 stored=“true” and multiValued=“false”. 主要作用就是不在内存中缓存大字段 | true、false | false  |

注意：

> - 上述属性是Field字段本身就有的
> - Field字段中有一个属性type，该属性会指向一个FieldType标签，该标签内部也会有上述描述的那些属性
> - **如果Field中某个属性在引用的FieldType中也存在，则以Field中定义的属性为准**
> - 一般来说，使用FieldType中定义的属性就已经够用了，如果字段比较特殊，可以在字段层面定义属性，用来覆盖掉FieldType中的属性

### FieldType详解

作用：定义在**索引**时该如何分词、索引、存储字段，在**查询**时该如何对查询串分词

```xml
<fieldType name="string" class="solr.StrField" sortMissingLast="true" docValues="true" />
<fieldType name="boolean" class="solr.BoolField" sortMissingLast="true"/>
<fieldType name="text_ws" class="solr.TextField" positionIncrementGap="100">
  <analyzer>
      <tokenizer class="solr.WhitespaceTokenizerFactory"/>
  </analyzer>
</fieldType>
```

字段类型属性说明：

> - name：字段类型名称，用于Field定义中的type属性引用
> - class：存放该类型的值来进行索引的字段类名（同lucene中Field的子类）。注意，应以 *solr.*为前缀，这样solr就可以很快定位到该到哪个包中去查找类，如 solr.TextField 。如果使用的是第三方包的类，则需要用全限定名。solr.TextField 的全限定名为：org.apache.solr.schema.TextField。
> - positionIncrementGap：用于多值字段，定义多值间的间隔，来阻止假的短语匹配
> - autoGeneratePhraseQueries：用于文本字段，如果设为true，solr会自动对该字段的查询生成短语查询，即使搜索文本没带“”
> - synonymQueryStyle：同义词查询分值计算方式
> - enableGraphQueries：是否支持图表查询
> - docValuesFormat：docValues字段的存储格式化器：schema-aware codec，配置在solrconfig.xml中的
> - postingsFormat：词条格式器：schema-aware codec，配置在solrconfig.xml中的

还有一些属性没有列举出来，这些字段同Field字段的属性是共有的。
**Solr中提供的 FieldType 类，在 org.apache.solr.schema 包下**
Solr自带的FieldType查看：http://lucene.apache.org/solr/guide/7_5/field-types-included-with-solr.html

#### FieldType的Analyzer

对于 solr.TextField 或 solr.SortableTextField 字段类型，需要为其定义分析器，有两种方式。
方式一：直接通过class属性指定分析器类，该类必须继承org.apache.lucene.analysis.Analyzer

```xml
<fieldType name="nametext" class="solr.TextField">
  <analyzer class="org.apache.lucene.analysis.core.WhitespaceAnalyzer"/>
</fieldType>
```

方式二：可以灵活地组合分词器、过滤器

```xml
<fieldType name="nametext" class="solr.TextField">
  <analyzer>
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.StandardFilterFactory"/>
    <filter class="solr.LowerCaseFilterFactory"/>
    <filter class="solr.StopFilterFactory"/>
  </analyzer>
</fieldType>
```

如果该类型字段索引、查询时需要使用不同的分析器，则需区分配置analyzer

```xml
<fieldType name="nametext" class="solr.TextField">
  <analyzer type="index">
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.LowerCaseFilterFactory"/>
    <filter class="solr.KeepWordFilterFactory" words="keepwords.txt"/>
    <filter class="solr.SynonymFilterFactory" synonyms="syns.txt"/>
  </analyzer>
  <analyzer type="query">
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.LowerCaseFilterFactory"/>
  </analyzer>
</fieldType>
```

Solr中提供的分词器tokenizer：http://lucene.apache.org/solr/guide/7_5/tokenizers.html
Solr中提供的过滤器filter：http://lucene.apache.org/solr/guide/7_5/filter-descriptions.html

#### 常用的Filter

- StopFilter：停用过滤器

  ```xml
  <analyzer>
  	<tokenizer class="solr.StandardTokenizerFactory"/>
  	<filter class="solr.StopFilterFactory" words="stopwords.txt"/>
  </analyzer>
  ```

  > - words属性指定停用词文件的绝对路径或相对solr主目录下conf/目录的相对路径
  > - 停用词定义语法：一行一个

- SynonymGraphFilter：同义词过滤器

  ```xml
  <analyzer type="index">
  	<tokenizer class="solr.StandardTokenizerFactory"/>
  	<filter class="solr.SynonymGraphFilterFactory" synonyms="mysynonyms.txt"/>
  	<filter class="solr.FlattenGraphFilterFactory"/>
  </analyzer>
  <analyzer type="query">
   	<tokenizer class="solr.StandardTokenizerFactory"/>
  	<filter class="solr.SynonymGraphFilterFactory" synonyms="mysynonyms.txt"/>
  </analyzer>
  ```

  > - synonyms属性指定同义词文件的绝对路径或相对solr主目录下conf/目录的相对路径
  > - 同义词定义语法：一类一行，如下表示，=>表示查询时标准化为后面的内容
  >   couch,sofa,divan
  >   teh => the
  >   huge,ginormous,humungous => large
  >   small => tiny,teeny,weeny