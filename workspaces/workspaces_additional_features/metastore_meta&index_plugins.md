As explained in the previous chapter, a workspace is mainly configured by 1/ its access driver (how do I access the actual data?) and 2/ additional plugins (enriching the data & features). This second part is handled by various plugins that are all of a specific type : metastore, meta,  and index. This order is not a random one : when adding a plugin you will notice that Pydio will always reorder them like that. All these aspects will generally handle not the “data” itself (your files, their content), but “metadata”, that is additionnal data describing the files. This data can be inherent to the files (e.g., the filesize can actually be seen as a metadata), or external, generated by the users (user-defined metadata) or by any system (a revision number from a versioning system).

So to take them in the order :

+ **Metastore**:
    - Defines how the metadata will be stored for the current workspace.  This can be linked to the underlying access driver but not necessarily.
    - Always the first in the stack, as meta & index plugins will often actually depend on a metastore.  Other features (like sharing and bookmarks) makes use of this feature to provide a better user experience as well. It it thus always recommanded to add at least a metastore to each workspace.
    - Current implementations include metastore.serial (stores files metadata on local filesystem, inside hidden files), metastore.s3 (to be used with access.s3, uses Amazon metadata capabilities) and metastore.xattr (uses file system Extended attributes, still experimental).
    - Generally if you are not sure, metastore.serial will do the job.
+ **Meta**:
    - One or many meta plugins can be added, they will actually implement the real features, and generally will require a metastore to be present for storing/retrieving their custom metadata.
    - Can go from user-defined metadata to quota implementation or versionning. See following sections of this manual.
+ **Index**
    - Indexation plugins, only Lucene is currently bundled by default, should be sufficient.
    - Always appears at the end of the stack, as other plugins can register metadata to be indexed by the search engine.

    [:image-popup:metastore_meta_index_plugins/08-ADDITIONAL-FEATURES-WORKSPACES.png]

    In the next sections, we will go through the common features one wants to implement on the workspaces. Please check the plugins documentations for detailed instruction and additional available features.