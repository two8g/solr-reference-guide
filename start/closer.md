# 深入一步

你已经对Solr的架构有点概念了，这节介绍一下Solr的home目录和其它配置选项。

当Solr在应用服务中运行，它必须能够访问homr目录。home目录包含重要的配置信息，也是Solr存放索引的地方。

目录的关键部分如下：

```
<solr-home-directory>/
    solr.xml
    conf/
        solrconfig.xml
        schema.xml
    data/
```