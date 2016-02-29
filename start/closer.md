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

`solr.xml`指定Solr core的配置选项，同时支持自定义配置多个cores.更多内容请查看[Slolr cores和solr.xml](Slolr cores和solr.xml.md)

You supply solr.xml , solrconfig.xml , and schema.xml to tell Solr how to behave. By default, Solr stores its
index inside data.
solr.xml specifies configuration options for your Solr core, and also allows you to configure multiple cores. For
more information on solr.xml see Solr Cores and solr.xml .
solrconfig.xml controls high-level behavior. You can, for example, specify an alternate location for the data
directory. For more information on solrconfig.xml , see Configuring solrconfig.xml .
schema.xml describes the documents you will ask Solr to index. Inside schema.xml , you define a document as a
collection of fields. You get to define both the field types and the fields themselves. Field type definitions are
powerful and include information about how Solr processes incoming field values and query values. For more
information on schema.xml , see Documents, Fields, and Schema Design .