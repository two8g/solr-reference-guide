# 运行

本节介绍如何运行简单schema的Solr,添加文档,运行查询.

## 启动服务

如果你安装Solr之后没有启动,你可以运行Solr目录下的bin/solr可执行文件.

    $ bin/solr start

如果你使用Windows操作系统,你可以运行bin\solr.cmd来启动 Solr.

    bin\solr.cmd start

这将在后台运行Solr,并监听8983端口.

当后台启动Solr时,脚本在返回命令行前快速启动Solr.

脚本bin/solr和 bin\solr.cmd可以自定义Solr运行模式.我们来练习一些使用bin/solr脚本的例子(如果在Windows下运行,bin\solr.cmd和下面例子的使用方式一样):

### Solr脚本选项

bin/solr脚本拥有多个选项.

### *脚本帮助*

查看使用bin/solr脚本说明,运行:

    $ bin/solr --help

### *前台运行Solr*

由于Solr是一个服务,通常运行于后台程序,特别是在Unix/Linux系统上.而在前台运行,只需运行:

    $ bin/solr start -f
如果是Windows,运行:

    $ bin\solr.cmd start -f

###