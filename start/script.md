# Solr启脚本介绍

Solr的脚本"/bin/solr"，用来启动和关闭solr，创建和删除collections或core, 检查Solr的状态和配置分片。该脚本在Solr安装目录的bin文件夹下。 `bin/solr`脚本方便使用简单命令和选项快速完成基本操作。

这一节，一一介绍下面列出的可使用命令。每个命令的介绍都附带示例。

这本指南包含`bin/solr`在使用中的很多示例，不过这节着重讲解 Solr 的运行和SolrCloud的开始
More examples of bin/solr in use are available throughout the Solr Reference Guide, but particularly in the sections
Running Solr and Getting Started with SolrCloud .   

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