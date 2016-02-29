# 深入一步

You already have some idea of Solr's schema. This section describes Solr's home directory and other configuration
options.
你已经对Solr的架构有点概念了，这节介绍一下Solr的home目录和其它配置选项。
When Solr runs in an application server, it needs access to a home directory. The home directory contains important
configuration information and is the place where Solr will store its index.
当Solr在应用服务中运行，它必须能够访问homr目录。home目录包含重要的配置信息，也是Solr存放索引的地方。
The crucial parts of the Solr home directory are shown here:
Solr home目录的关键部分如下：

```
<solr-home-directory>/
solr.xml
conf/
solrconfig.xml
schema.xml
data/
```