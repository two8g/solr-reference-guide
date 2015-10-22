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

#### *IndexBasedSpellChecker*

IndexBasedSpellChecker 基于一个 Solr 索引作为相似的索引用于拼写检查.
它需要定义一个field作为索引词元的基础;a common practice is to copy terms from some fields (such
as title , body , etc.) to another field created for spell checking. Here is a simple example of configuring solrconfig.xml with the IndexBasedSpellChecker :

    <searchComponent name="spellcheck" class="solr.SpellCheckComponent">
        <lst name="spellchecker">
            <str name="classname">solr.IndexBasedSpellChecker</str>
            <str name="spellcheckIndexDir">./spellchecker</str>
            <str name="field">content</str>
            <str name="buildOnCommit">true</str>
        </lst>
    </searchComponent>

第一个元素定义 *searchComponent* 使用 *solr.SpellCheckComponent*. *classname*表示SpellCheckComponent的实现类，这里使用*solr.IndexBasedSpellChecker*.
*classname*是可选项,如不定义,默认使用*IndexBasedSpellChecker*.

*spellcheckIndexDir*定义支持拼写检查索引的目录位置,*field*定义拼写检查词元的字段源,
最好不要选择获取想要的结果时处理过程复杂的字段.If the field has many word variations
from processing synonyms and/or stemming, the dictionary will be created with those variations in addition to more valid spelling data.

最后的*buildOnCommit*定义是否在每次索引的commit时构建拼写检查索引,它是可选,默认为false.

#### *DirectSolrSpellCheker*

The DirectSolrSpellChecker uses terms from the Solr index without building a parallel index like the IndexBa
sedSpellChecker . This spell checker has the benefit of not having to be built regularly, meaning that the terms
are always up-to-date with terms in the index. Here is how this might be configured in solrconfig.xml

    <searchComponent name="spellcheck" class="solr.SpellCheckComponent">
        <lst name="spellchecker">
            <str name="name">default</str>
            <str name="field">name</str>
            <str name="classname">solr.DirectSolrSpellChecker</str>
            <str name="distanceMeasure">internal</str>
            <float name="accuracy">0.5</float>
            <int name="maxEdits">2</int>
            <int name="minPrefix">1</int>
            <int name="maxInspections">5</int>
            <int name="minQueryLength">4</int>
            <float name="maxQueryFrequency">0.01</float>
            <float name="thresholdTokenFrequency">.01</float>
        </lst>
    </searchComponent>
    
## <span id="parameters">参数</span>