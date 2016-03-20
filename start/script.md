# Solr启脚本介绍

Solr的脚本"/bin/solr"，用来启动和关闭solr，创建和删除collections或core, 检查Solr的状态和配置分片。该脚本在Solr安装目录的bin文件夹下。 `bin/solr`脚本方便使用简单命令和选项快速完成基本操作。
Solr includes a script known as "/bin/solr" that allows you to start and stop Solr, create and delete collections or
cores, and check the status of Solr and configured shards. You can find the script in the bin directory of your Solr
installation. The bin/solr script makes Solr easier to work with by providing simple commands and options to quickly
accomplish common goals.

这一节，一一介绍下面列出的可使用命令。每个命令的介绍都附带示例。
In this section, the headings below correspond to available commands. For each command, the available options
are described with examples.

这本指南包含bin/solr在使用中的很多示例，
More examples of bin/solr in use are available throughout the Solr Reference Guide, but particularly in the sections
Running Solr and Getting Started with SolrCloud .   

* 启动和关闭
    * 启动和重启
    * 状态
    * 停止
    * 健康检查
* Collections 和 Cores
    * 创建
    * 删除

启动和关闭