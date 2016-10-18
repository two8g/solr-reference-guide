# 更新Solr

If you are already using Solr 4.10, Solr 5.0 should not present any major problems. However, you should review the
CHANGES.txt file found in your Solr package for changes and updates that may effect your existing
implementation.

## Upgrading from 4.10.x

* Solr is no longer distributed as a "war" (Web Application Archive) suitable for deployment in any Servlet
Container. Details of the motivation for this change, as well as instructions for Upgrading a Solr 4.x Cluster to
Solr 5.0 can be found in the Major Changes from Solr 4 to Solr 5 section.
* Solr no longer supports reading Lucene/Solr 3.x and earlier indexes. See the Major Changes from Solr 4 to
Solr 5 section of this guide for details on how to convert older indexes to the current format.
* The 'old-style' solr.xml format deprecated in Solr 4.3 is no longer supported, and cores must be defined using
core.properties files. Some instructions for Moving to the New solr.xml Format are included with this guide.
* The " file " attribute of infoStream in solrconfig.xml is no longer supported. Control this via your
logging configuration (org.apache.solr.update.LoggingInfoStream) instead.
* UniqFieldsUpdateProcessorFactory no longer supports the <lst named="fields"> init param
style that was deprecated in Solr 4.5. If you are still using this syntax, update your configs to use <arr
name="fieldName"> instead. See SOLR-4249 for more details.
* The following legacy numeric and date field types, deprecated in Solr 4.8, are no longer supported: BCDIntF
ield , BCDLongField , BCDStrField , IntField , LongField , FloatField , DoubleField , SortableI
ntField , SortableLongField , SortableFloatField , SortableDoubleField , and DateField .
* Convert these types in your schema to the corresponding Trie-based field type and then re-index. See SOLR
-5936 for more information.
getAnalyzer() in IndexSchema and FieldType that was deprecated in Solr 4.9 has been removed.
* Use getIndexAnalyzer() instead. See SOLR-6022 for more information.
* The spellcheck response format has changed, affecting xml and json clients. In particular, the "correctlyS
pelled " and " collations " subsections have been moved outside the " suggestions " subsection, and
now are directly under " spellcheck ". See SOLR-3029 for more information.
* The OVERSEERSTATUS API returns new key names for operations such as " create " for " createcollec
tion ", " delete " for " removecollection " and " deleteshard " for " removeshard ".
If you have been using the /update/json/docs to index documents, SOLR-6617 introduces backward
incompatible change. the key names created are fully qualified paths of keys. If you need the old functionality,
please add an extra parameter f=/** For example: /update/json/docs?f=/**
* Bugs fixed in several ValueSource functions may result in different behavior in situations where some
documents do not have values for fields wrapped in other value sources. Users who want to preserve the
previous behavior may need to wrap fields in the " def() " function. Example: changing " fl=sum(fieldA,f
ieldB) " to " fl=sum(def(fieldA,0.0),def(fieldB,0.0)) ". See LUCENE-5961 for more details.
* AdminHandlers is deprecated, /admin/* are implicitly defined, /get , /replication handlers are also
implicitly registered (refer to SOLR-6792 )
* The " termIndexInterval " option in solrconfig.xml has been a No-Op in the default codec since Solr
4.0, and has been removed completely in 5.0. If you get an "Illegal parameter
'termIndexInterval' " error when upgrading, you can safely remove this option from your configs. If you
have a strong need to configure this, you must explicitly configure your schema with a custom codec. See S
OLR-6560 and for more details.
* The " checkIntegrityAtMerge " option in solrconfig.xml is now a No-Op and should be removed from
23any solrconfig.xml files -- these integrity checks are now done automatically at a very low level during
the segment merging process. See SOLR-6834 for more details.
* SimplePostTool ( post.jar ) no longer defaults to collection1 , making either of core/collection name or
update URL mandatory. An existing call without an explicit update URL needs to now have the core/collection
name passed as " -Dc=<collection/core name> " e.g.: java -Dc=<collection_name> -jar post
.jar *.xml (new call with collection name). See SOLR-6852 for more details.
* Relative paths specified in the solr.xml coreRootDirectory parameter for core discovery are now
resolved relative to SOLR_HOME , rather than cwd. See SOLR-6718 .
* SolrServer and associated classes have been deprecated. Applications using SolrJ should use the
equivalent SolrClient classes instead.
* Spatial fields originating from Solr 4 (e.g. SpatialRecursivePrefixTreeFieldType , BBoxField ) have
the ' units ' attribute deprecated, now replaced with ' distanceUnits '. If you change it to a unit other than '
degrees ' (or if you don't specify it, which will default to kilometers if geo=true ), then be sure to update m
axDistErr as it's in those units. If you keep units=degrees then it should be backwards compatible but
you'll get a deprecation warning on startup. See SOLR-6797 .
* The <nrtMode> configuration in solrconfig.xml has been discontinued and should be removed from so
lrconfig.xml . Solr defaults to using NRT searchers regardless of the value in configuration and a warning
is logged on startup if the solrconfig.xml has <nrtMode> specified.
* There was an old spatial syntax to specify a circle using Circle(x,y d=...) which should be replaced
with simply using {!geofilt} (if you can) or BUFFER(POINT(x y),d) . Likewise a rect syntax comprised
of minX minY maxX maxY that should now be replaced with ENVELOPE(minX, maxX, maxY, minY) .
* Due to changes in the underlying commons-codec package, users of the BeiderMorseFilterFactory will
need to rebuild their indexes after upgrading. See LUCENE-6058 for more details.
* CachedSqlEntityProcessor has been removed, use SqlEntityProcessor with the cacheImpl para
meter.
* HttpDataSource has been removed, use URLDataSource instead.
* The deprecated LegacyHTMLStripCharFilter has been removed.
* CoreAdminRequest.persist() call has been removed. All changes made via CoreAdmin are persistent.
* Many deprecated methods have been removed from various SolrJ classes. Please consult the Solr 4.10
javadocs for the SolrJ package for information regarding the new supported methods.

## Upgrading from Older Versions of Solr

Users upgrading from older versions are strongly encouraged to consult CHANGES.txt for the details of all changes
since the version they are upgrading from, a summary of the significant changes can be found in the Major Changes
from Solr 4 to Solr 5 .