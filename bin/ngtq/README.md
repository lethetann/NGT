NGTQ
===

Neighborhood Graph and Tree for Indexing High-dimensional Data with Quantization

Command
=======



**ngtq** - proximity search for billions of high dimensional data



      $ ngtq command [option] index [data]
        
**Note:**

When the environment variable POSIXLY_CORERECT is set on some platforms such as Cygwin, you should specifiy options 
before the command as follows.

      $ ngtq [option] command index [data]



**ngtq** provides high-speed nearest neighbor searches against billions of data in high dimensional vector data space (several ten to several thousand dimensions).

**command** is one of:

-   *[create](#create)*
-   *[append](#append)*
-   *[search](#search)*

### CREATE

Constructs the specified index with the specified data.

      $ ngtq create -d no_of_dimensions [-p no_of_threads] [-o object_type] [-n no_of_registration_data] 
          [-C global_codebook_size] [-c local_codebook_size] [-N no_of_divisions] 
          [-L local_centroid_creation_mode] 
          index registration_data

*index*  
Specifies the name of the directory for the index to be generated. After data registration, the directory consists of multiple files for the index.

*registration\_data*  
Specifies the vector data to be registered. These data shall consist of one object (data item) per line and each dimensional element shall be delimited by a space or tab. Note that L2 is only avilable as the distance function.

**-d** *no\_of\_dimensions*  
Specifies the number of dimensions of registration data.

**-p** *no\_of\_threads* (default = 24, recomended value = number of cores)   
Specifies the number of threads to be used for parallel processing at generation time.

**-o** *object\_type*  
Specifies the data object type.
- __c__: 1 byte unsigned integer (not fully implemented)
- __f__: 4 byte floating point number (default/recommended)

**-n** *no\_of\_registration\_data*  
Specifies the number of data items to be registered. If not specified, all data in the specified file will be registered.

**-C** *global\_codebook\_size*  
Specifies the number of the global codes (centroids).

**-c** *local\_codebook\_size*  
Specifies the number of the local codes (centroids).

**-N** *no\_of\_divisions*  
Specifies the number of division of the vector data for the local vector data (residual data).

**-L** *local\_centroid\_creation\_mode*  
Specifies the creation mode for the local centroids.
- __d__: The heads of the specified registration data are used as the local centroids.
- __k__: The local centoroids are generated by using kmeans.


### APPEND

Adds the specified data to the specified index.

      $ ngtq append [-n no_of_registration_data] index registration_data
        

*index*  
Specifies the name of the existing index.

*registration\_data*  
Specifies the vector data to be registered. These data shall consist of one object (data item) per line and each dimensional element shall be delimited by a space or tab.

**-n** *no\_of\_registration\_data*  
Specifies the number of data items to be registered. If not specified, all data in the specified file will be registered.

### SEARCH

Searches the index using the specified query data.

      $ ngtq search [-n no_of_search_results] [-e search_range_coefficient] [-m mode] 
          [-r search_radius] [-E approximate-expansion] index query_data
        

*index*  
Specifies the name of the existing index.

*query\_data*  
Specifies the name of the file containing query data. This file shall consist of one item of query data per line and each dimensional element of that data item shall be delimited by a space or tab the same as registration data. Each search shall be sequentially performed when providing multiple queries.

**-n** *no\_of\_search\_results* (default: 20)  
Specifies the number of search results.

**-e** *search\_range\_coefficient* (default = recomended value = 0.1)  
Specifies the magnification coefficient of the search range to search for the global codebook. A larger value means greater accuracy but slower searching, while a smaller value means a drop in accuracy but faster searching. While it is desirable to adjust this value within the range of 0 - 0.3, a negative value may also be specified.

**-m** *search\_mode* (__r__|__e__|__l__|__c__|__a__)  
Specifies the search mode.
- __a__: searches using approximate distances.
- __c__: searches using approximate distances. Caching computed local distances reduces the query time. (recommended)
- __l__: searches using approximate distances with local distance lookup tables.
- __e__: searches using exact distances. The local codebooks are not used.
- __r__: searches using exact distances after screening by approximate distances. (recommended if you need exact distances)

**-E** *approximate\_expansion*  
Specifies the expansion ratio of the number of approximate search results to the number of search results. For example, when the retio is 10 and the number of search results is 20, the number of the approximate search results is set to 200.

