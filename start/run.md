# 运行

本节介绍如何运行简单schema的Solr,添加文档,运行查询.

##启动服务

如果你安装 Solr 之后没有启动, 你可以运行 Solr 目录下的 bin/solr 可执行文件.

    $ bin/solr start

如果你使用 Windows 操作系统, 你可以运行 bin\solr.cmd 来启动 Solr.

    bin\solr.cmd start

这将在后台运行 Solr, 并监听8983端口.

当后台启动 Solr 时, 脚本在返回命令行前快速启动 Solr.

脚本 bin/solr 和 bin\solr.cmd 可以自定义 Solr 运行模式. 