# 拼写检查

拼写检查(SpellCheck)组件被设计用于借助其它相似的词元(terms)提供在线查询建议(Suggester)
这种建议的基础可以是Solr中某个field中的terms,外部已有文本文件,或者其它Lucene索引的fileds.

本节主题列表
* [配置 SpellCheckComponent](#configure)
* [Spell Check 参数](#parameters)
* [分布式 SpellCheck](#distributed)



## <span id="configure" name="configure">配置 SpellCheckComponent</span>

### 在 solrconfig.xml 中定义 Spell Check

第一步, 在 soleconfig.xml 中指定terms的来源, Solr 提供三个来源, 讨论如下.

