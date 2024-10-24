To prevent getting billed for usage beyond the free courtesy usage limits, you can set requests per day caps:
https://cloud.google.com/apis/docs/capping-api-usage

Create BigLake external tables for Cloud Storage:
https://cloud.google.com/bigquery/docs/create-cloud-storage-table-biglake#console

Cloud Storage
free-tier:
- 5 GB-months of regional storage (US regions only) per month
- 5,000 Class A Operations per month
- 50,000 Class B Operations per month
- 100 GB of outbound data transfer from North America to all region destinations (excluding China and Australia) per month

Cloud Storage allows world-wide storage and retrieval of any amount of data at any time. You can use Cloud Storage for a range of scenarios including serving website content, storing data for archival and disaster recovery, or distributing large data objects to users via direct download.

Develop and deploy data pipelines and storage to analyze large amounts of data. Cloud Storage offers high availability and performance while being strongly consistent, giving you confidence and accuracy in analytics workloads.

<img width="968" alt="image" src="https://github.com/user-attachments/assets/55704e3b-ba80-420c-ba25-a684f8790abf">

https://cloud.google.com/blog/products/storage-data-transfer/hdfs-vs-cloud-storage-pros-cons-and-migration-tips

### HDFS vs cloud-storage

Cons:

+ Cloud Storage may increase I/O variance
  + In many situations, Cloud Storage has a higher I/O variance than HDFS.
  + This can be problematic if you have consistent I/O requirements, such as an application backed by HBase or another NoSQL database.
+ Cloud Storage does not support file appends or truncates
  + Objects are immutable, which means that an uploaded object cannot change throughout its storage lifetime.
  + In practice, this means that you cannot make incremental changes to objects, such as append operations or truncate operations.
+ Cloud Storage is not POSIX-compliant
+ Cloud Storage may not expose all file system information
  + Cloud Storage abstracts the entire management layer of all storage
+ Cloud storage may have greater request latency
  + Cloud storage may have a greater request round-trip latency compared to HDFS.

Pros:

+ Lower costs
+ Separation from compute and storage
  + When you store data in Cloud Storage instead of HDFS, you can access it directly from multiple clusters.
+ Interoperability
  + Storing data in Cloud Storage enables seamless interoperability between Spark and Hadoop instances as well as other GCP services
+ HDFS compatibility with equivalent (or better) performance
  + You can access Cloud Storage data from your existing Hadoop or Spark jobs simply by using the **gs://** prefix instead of **hfds:://**.
+ High data availability
  + Data stored in Cloud Storage is highly available and globally replicated (when using multi-regional storage) without a loss of performance.
  + Cloud Storage is not vulnerable to NameNode single point of failure or even cluster failure.
+ No storage management overhead
  + Unlike HDFS, Cloud Storage requires no routine maintenance such as running checksums on the files, upgrading or rolling back to a previous version of the file system and other administrative tasks.
+ Quick startup
  + With Cloud Storage, you can start your jobs as soon as the task nodes start
  + In HDFS, a MapReduce job can’t start until the NameNode is out of safe mode—a process that can take from a few seconds to many minutes, depending on the size and state of your data
+ Google IAM security
+ Global consistency
  + Cloud Storage provides strong global consistency for the below operations; this includes both data and metadata.
    + Read-after-write
    + Read-after-metadata-update
    + Read-after-delete
    + Bucket listing
    + Object listing
    + Granting access to resources

Cloud Storage -> Cloud Storage is a managed service for storing unstructured data. Store any amount of data and retrieve it as often as you like.

BigQuery -> BigQuery's serverless architecture lets you use SQL queries to analyze your data. You can store and analyze your data within BigQuery or use BigQuery to assess your data where it lives. To test how it works for yourself, query data—without a credit card—using the BigQuery sandbox.

BigQuery is a cloud-based big data analytics web service for processing very large data sets. BigQuery was designed for analyzing data on the order of billions of rows, using a SQLlike syntax.

BigQuery, MapReduce and data ware house are fundamentally different technologies and each has different use cases:

<img width="476" alt="image" src="https://github.com/user-attachments/assets/db876e6f-6661-4674-82ea-b0c9ded8a188">
