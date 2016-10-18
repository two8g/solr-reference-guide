# 深入一步

你已经对Solr的架构有点概念了，这节介绍一下Solr的home目录和其它配置选项。

当Solr在应用服务中运行，它必须能够访问homr目录。home目录包含重要的配置信息，也是Solr存放索引的地方。

Solr home目录的关键部分如下：

```
<solr-home-directory>/
    solr.xml
    conf/
        solrconfig.xml
        schema.xml
    data/
```

你必须提供`solr.xml`,`solrconfig.xml` 和`schema.xml`这三个文件告诉Solr如何工作.Solr默认将索引数据存储在data目录下。

`solr.xml`指定Solr core的配置选项，同时支持自定义配置多个cores.更多内容请查看[Slolr cores和solr.xml](/configure/cores-solr.md).

`solrconfig.xml`控制高级行为。例如，你可以指定一个替代data目录的路径.更多内容在[配置solrconfig.xml](/configure/solrconfig.md)

`schema.xml`描述Solr建立索引的文档格式。其中，你可以定义文档为多个field的一个集合。你需要同时定义field的type和field本身。Field Type的定义很强大，包括Solr对输入的field的处理过程和如何搜索。请查看[Documents,Fields和Schema设计](/document-field-schema.md)。