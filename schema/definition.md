# Field 类型定义和属性

A field type definition can include four types of information:

* The name of the field type (mandatory)
* An implementation class name (mandatory)
* If the field type is `TextField` , a description of the field analysis for the field type
* Field type properties - depending on the implementation class, some properties may be mandatory.

## Field Type Definitions in schema.xml

Field types are defined in `schema.xml` . Each field type is defined between `fieldType` elements. They can optionally be grouped within a `types` element. Here is an example of a field type definition for a type called `text_general` :

```xml
<fieldType name="text_general" class="solr.TextField" positionIncrementGap="100">
    <analyzer type="index">
        <tokenizer class="solr.StandardTokenizerFactory"/>
        <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" />
        <!-- in this example, we will only use synonyms at query time
        <filter class="solr.SynonymFilterFactory" synonyms="index_synonyms.txt"
        ignoreCase="true" expand="false"/>
        -->
        <filter class="solr.LowerCaseFilterFactory"/>
    </analyzer>
    <analyzer type="query">
        <tokenizer class="solr.StandardTokenizerFactory"/>
        <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" />
        <filter class="solr.SynonymFilterFactory" synonyms="synonyms.txt"
            ignoreCase="true" expand="true"/>
        <filter class="solr.LowerCaseFilterFactory"/>
    </analyzer>
</fieldType>
```
The first line in the example above contains the field type name, `text_general` , and the name of the implementing class, `solr.TextField` . The rest of the definition is about field analysis, described in Understanding Analyzers, Tokenizers, and Filters[分析器,分词器和过滤器](../analyzer-tokenizer-filter.md).

The implementing class is responsible for making sure the field is handled correctly. In the class names in `schema.xml` , the string `solr` is shorthand for `org.apache.solr.schema` or `org.apache.solr.analysis` . Therefore, solr.TextField is really `org.apache.solr.schema.TextField`.

## Field Type Properties

The field type `class` determines most of the behavior of a field type, but optional properties can also be defined.For example, the following definition of a date field type defines two properties, `sortMissingLast` and `omitNorms` .

```xml
<fieldType name="date" class="solr.TrieDateField"
           sortMissingLast="true" omitNorms="true"/>
```

The properties that can be specified for a given field type fall into three major categories:

* Properties specific to the field type's class.
* [General Properties](#_general-properties_) Solr supports for any field type.
* [Field Default Properties](#_field-default-properties_) that can be specified on the field type that will be inherited by fields that use this type instead of the default behavior.

## _General Properties_

| Property                  | Description | Values        |
|:--------------------------|:------------|:--------------|
| name                      |   The name of the fieldType. This value gets used in field definitions, in the "type" attribute. It is strongly recommended that names consist of alphanumeric or underscore characters only and not start with a digit. This is not currently strictly enforced.       |               |
| class                     |   The class name that gets used to store and index the data for this type. Note that you may prefix included class names with "solr." and Solr will automatically figure out which packages to search for the class - so "solr.TextField" will work. If you are using a third-party class, you will probably need to have a fully qualified class name. The fully qualified equivalent for "solr.TextField" is "org.apache.solr.schema.TextField".         |               |
| positionIncrementGap      |   For multivalued fields, specifies a distance between multiple values, which prevents spurious phrase matches          | integer       |
| autoGeneratePhraseQueries |    For text fields. If true, Solr automatically generates phrase queries for adjacent terms. If false, terms must be enclosed in double-quotes to be treated as phrases.         | true or false |
| docValuesFormat           |   Defines a custom `DocValuesFormat` to use for fields of this type. This n/a requires that a schema-aware codec, such as the `SchemaCodecFactory` has been configured in solrconfig.xml.         | n/a           |
| postingsFormat            |    Defines a custom `PostingsFormat` to use for fields of this type. This n/a requires that a schema-aware codec, such as the `SchemaCodecFactory` has been configured in solrconfig.xml.         | n/a           |

![Info](../info.png)Lucene index back-compatibility is only supported for the default codec. If you choose to customize the pos
tingsFormat or docValuesFormat in your schema.xml, upgrading to a future version of Solr may
require you to either switch back to the default codec and optimize your index to rewrite it into the default
codec before upgrading, or re-build your entire index from scratch after upgrading.

## _Field Default Properties_