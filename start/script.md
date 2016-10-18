# Solr启脚本介绍

Solr的脚本"/bin/solr"，用来启动和关闭solr，创建和删除collections或core, 检查Solr的状态和配置分片。该脚本在Solr安装目录的bin文件夹下。 `bin/solr`脚本方便使用简单命令和选项快速完成基本操作。

这一节，一一介绍下面列出的可使用命令。每个命令的介绍都附带示例。

这本指南包含`bin/solr`在使用中的很多示例，不过这节着重讲解 Solr 的运行和SolrCloud的开始。

* [启动和关闭](#启动和关闭)
    * 启动和重启
    * 状态
    * 停止
    * 健康检查
* Collections 和 Cores
    * 创建
    * 删除

### 启动和关闭
#### 启动和重启

启动命令用来启动新的Solr服务，重启命令可以是正在运行的Solr服务重启或启动已停止的Solr。

启动和重启命令拥有一些选项，或用来启动SolrCloud，或使用一个示例配置，或指定自定义zookeeper服务启动。

```
./bin/solr start [options]
```
```
./bin/solr start -help
```
```
./bin/solr restart [options]
```
```
./bin/solr restart -help
```
使用重启命令时，必须通过指定你启动时使用的所指定参数。底层背后，重启会先开始一个停止服务请求，这样服务才可以重新启动。如果没有如何在运行的服务，重启请求会跳过关闭服务的过程直接启动。

#### 参数

`bin/solr`脚本包含很多通过普通方式自定义服务的选项,例如设置监听端口.其实,大部分默认设置都已经足够使用,特别是刚开始的使用.

| Parameter   | Description  | Example  |
|:---|:--|:--|
|  -a "<string>"  | 启动Solr时的JVM参数, 例如那些"-X"开头的参数. 如果你的参数开头是"-D",你可以省略`-a`选项.  |  ./bin/solr start -a "-Xdebug -Xrunjdwp:transport=dt_socket, server=y,suspend=n,address=1044" |
|   -cloud    |   以SolrCloud模式启动Solr, 且Solr服务会内嵌ZooKeeper服务. 如果你已经启动了ZooKeeper服务用来替代内嵌ZooKeeper,那需要使用`-z`参数.更多内容查看[SolrCloud 模式](#SolrCloud模式)|   `./bin/solr start -c`   |
|   -d <dir>    |   设置服务目录, 默认为目录server( `$SOLR_HOME/server` ). 一般不修改该选项. 当同时运行多个Solr实例时,一般也是使用同一个服务目录,同时使用`-s`参数设置不同的Solr home目录. | `./bin/solr start -d newServerDir` |
| -e <name> | 使用样例配置启动Solr. 这些样例可以快速启动一般的Solr,或是具有某一特性的Solr.可选的有: <br/> <ul><li>cloud</li><li>techproducts</li><li>dih</li><li>schemaless</li></ul> 查看后面的[运行样例配置](#运行样例配置)一节了解更多样例配置的信息.| `./bin/solr start -e schemaless` |
| -f | 前台模式运行Solr,该选项当使用了`-e`选项是不可用. | `./bin/solr start -f` |
| -h <hostname> | 使用自定义主机域名启动Solr. 默认为'localhost'. | `./bin/solr start -h search.mysolr.com` |
| -m <memory> | 设置Solr启动的JVM堆栈大小min (-Xms) and max (-Xmx). | `./bin/solr start -m 1g` |
| -noprompt | 关闭提示. 将会使用默认设置. <br/> 如, 当使用 "cloud" 时, 会通过交互式回话指引设置SolrCloud集群选项.如果你想全部使用默认配置,只需使用`-noprompt`选项即可.| `./bin/solr start -e cloud -noprompt` |
| -p <port> | 设置服务端口, 默认使用端口'8983'. | `./bin/solr start -p 8655` |
| -s <dir> | 设置solr.solr.home系统配置; Solr会在该目录下创建文件或目录. 便于管理运行多个Solr实例,如果它们使用的相同服务目录(通过-d 参数设置). 一旦设置,此目录下必需包含一个solr.xml文件. 默认配置为`server/solr`<br/> 当运行样例(-e)时,该参数被忽略,并取决于样例的设置. | `./bin/solr start -s newHome` |
| -V | Start Solr with verbose messages from the start script. | `./bin/solr start -V` |
| -z <zkHost> | 设置ZooKeeper连接地址. 只有使用了`-c`启动SoleCloud模式下才支持. 如果没有设置,将使用内嵌ZooKeeper实例 | `./bin/solr start -c -z server1:2181,server2:2181` |

默认配置和它的实际工作方式的等价说明如下:
```./bin/solr start```
```./bin/solr start -h localhost -p 8983 -d server -s solr -m 512m```
如果默认配置不影响使用就无需设置所有选项.

#### 设置 Java System 配置

The bin/solr script will pass any additional parameters that begin with -D to the JVM, which allows you to set
arbitrary Java system properties. For example, to set the auto soft-commit frequency to 3 seconds, you can do:
```./bin/solr start -Dsolr.autoSoftCommit.maxTime=3000```

#### SolrCloud模式

选项`-c`和`-cloud`等价:
```./bin/solr start -c```
```./bin/solr start -cloud```

如果你指定了ZooKeeper连接地址, such as `-z 192.168.1.4:2181`, 那么Solr会连接ZooKeeper加入集群,如果没有指定,Solr会在端口port+1000上启动一个内嵌的ZooKeeper服务,如果Solr是运行再8983端口,则内嵌ZooKeeper将运行再9983端口.

**重要**: If your ZooKeeper connection string uses a chroot, such as `localhost:2181/solr` , then you need to bootstrap the /solr znode before launching SolrCloud using the bin/solr script. To do this, you need to use the zkcli.sh script shipped with Solr, such as:
```server/scripts/cloud-scripts/zkcli.sh -zkhost localhost:2181/solr -cmd bootstrap -solrhome server/solr```

当以SolrCloud模式启动时,交互式回话会指导你配置服务选项.更多关于SolrCloud启动的内容,阅读章节[SolrCloud入门指南](../solrcloud/start.md).

#### 运行样例配置

```./bin/solr start -e <name>```

<!--The example configurations allow you to get started quickly with a configuration that mirrors what you hope to
accomplish with Solr. The following examples are provided:-->
样例配置能让你快速启动一个你想要的配置完整的solr实例.Solr提供以下样例:

* **cloud**: 此样例在单机上启动一个具有1-4个节点的SolrCloud集群.启动时,会通过交互式会话指导你选择集群的节点数目,服务的端口,和要创建的collection的名称.

<!--This example starts a 1-4 node SolrCloud cluster on a single machine. When chosen, an interactive
session will start to guide you through options to select the number of nodes for your example cluster, the
ports to use, and name of the collection to be created.-->

* **techproducts**: 此样例以独立模式启动一个Solr,包含`$SOLR_HOME/example/exampledocs`目录中的文档及对应的schema设计.此配置集下SolrCloud或schemaless模式不可用,fields必需在`schema.xml`中定义来包含那些用来建索引的文档的fields.configset在目录`$SOLR_HOME/server/solr/configsets/sample_techproducts_configs`下.

<!_-This example starts Solr in standalone mode with a schema designed for the sample
documents included in the $SOLR_HOME/example/exampledocs directory. The configset used does not have SolrCloud or schemaless modes enabled, so fields must be explicitly defined in schema.xml in order for documents including those fields to be added to the index. The configset used can be found in $SOLR_HOME/server/solr/configsets/sample_techproducts_configs .-->

* **dih**: 

<!--This example starts Solr in standalone mode with the DataImportHandler (DIH) enabled and several
example dataconfig.xml files pre-configured for different types of data supported with DIH (such as,
database contents, email, RSS feeds, etc.). For more information about DIH, see the section Uploading
Structured Data Store Data with the Data Import Handler .-->

* **schemaless**: 
<!--This example starts Solr in standalone mode using a managed schema, as described in the
section Managed Schema Definition in SolrConfig , and provides a very minimal pre-defined schema. Solr will
run in Schemaless Mode with this configuration, where Solr will create fields in the schema on the fly and will
guess field types used in incoming documents. The configset used can be found in $SOLR_HOME/server/s
olr/configsets/data_driven_schema_configs .-->
