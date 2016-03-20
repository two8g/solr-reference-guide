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