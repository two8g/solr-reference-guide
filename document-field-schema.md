# Documents,Fields和Schema

This section discusses how Solr organizes its data into documents and fields, as well as how to work with the Solr
schema file, schema.xml . It includes the following topics:
[Documents,Fields与Schema概述](schema/overview.md): An introduction to the concepts covered in this section.
[Solr Field Type](schema/fieldtype.md): Detailed information about field types in Solr, including the field types in the default Solr schema.
[定义 Fields](schema/field.md): Describes how to define fields in Solr.
[拷贝 Fields](schema/copyfield.md): Describes how to populate fields with data copied from another field.
[动态 Fields](schema/dynamic.md): Information about using dynamic fields in order to catch and index fields that do not exactly conform
to other field definitions in your schema.
[Schema API](schema/api.md): Use curl commands to read various parts of a schema or create new fields and copyField rules.
[Schema元素](schema/element.md): Describes other important elements in the Solr schema.
[零件组装](schema/pieces.md): A higher-level view of the Solr schema and how its elements work together.
[DocValues](schema/doc-values.md): Describes how to create a docValues index for faster lookups.
[Schemaless模式](schema/schema-less.md): Automatically add previously unknown schema fields using value-based field type guessing.