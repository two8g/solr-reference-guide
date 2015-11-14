# 运行

本节介绍如何运行简单schema的Solr,添加文档,运行查询.

## 启动服务

如果你安装Solr之后没有启动,你可以运行Solr目录下的bin/solr可执行文件.

    $ bin/solr start

如果你使用Windows操作系统,你可以运行bin\solr.cmd来启动 Solr.

    bin\solr.cmd start

这将在后台运行Solr,并监听8983端口.

当后台启动Solr时,脚本在返回命令行前快速启动Solr.

脚本bin/solr和 bin\solr.cmd可以自定义Solr运行模式.