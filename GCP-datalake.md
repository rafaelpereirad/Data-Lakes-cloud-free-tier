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

BigQuery is a fully managed, AI-ready data analytics platform that helps you maximize value from your data and is designed to be multi-engine, multi-format, and multi-cloud.

<img width="617" alt="image" src="https://github.com/user-attachments/assets/f93b40a3-27cb-49f5-8479-9e99de263039">

| Services and Usage         | Subscription Type                                                                             | Price (USD)                                       |
|----------------------------|------------------------------------------------------------------------------------------------|---------------------------------------------------|
| **Free Tier**              | The BigQuery free tier gives customers 10 GiB storage, up to 1 TiB queries free per month, and other resources. | **Free**                                          |
| **Compute (Analysis)**     |                                                                                               |                                                   |
| On-demand                  | Generally gives you access to up to 2,000 concurrent slots, shared among all queries in a single project. | Starting at $6.25 per TiB scanned. First 1 TiB per month is free. |
| Standard Edition           | Low-cost option for standard SQL analysis                                                     | $0.04 per slot hour                               |
| Enterprise Edition         | Supports advanced enterprise analytics                                                        | $0.06 per slot hour                               |
| Enterprise Plus Edition    | Supports mission-critical enterprise analytics                                                | $0.10 per slot hour                               |
| **Storage**                |                                                                                               |                                                   |
| Active Local Storage       | Based on the uncompressed bytes used in tables or table partitions modified in the last 90 days. | Starting at $0.02 per GiB. The first 10 GiB is free each month. |
| Long-term Logical Storage  | Based on the uncompressed bytes used in tables or table partitions modified for 90 consecutive days. | Starting at $0.01 per GiB. The first 10 GiB is free each month. |
| Active Physical Storage    | Based on the compressed bytes used in tables or table partitions modified for 90 consecutive days. | Starting at $0.04 per GiB. The first 10 GiB is free each month. |
| Long-term Physical Storage | Based on compressed bytes in tables or partitions that have not been modified for 90 consecutive days. | Starting at $0.02 per GiB. The first 10 GiB is free each month. |
| **Data Ingestion**         |                                                                                               |                                                   |
| Batch Loading              | Import table from Cloud Storage                                                               | Free (when using the shared slot pool)            |
| Streaming Inserts          | Charged for rows that are successfully inserted. Individual rows are calculated using a 1 KB minimum. | $0.01 per 200 MiB                                 |
| BigQuery Storage Write API | Data loaded into BigQuery is subject to BigQuery storage pricing or Cloud Storage pricing.    | $0.025 per 1 GiB. The first 2 TiB per month are free. |
| **Data Extraction**        |                                                                                               |                                                   |
| Batch Export               | Export table data to Cloud Storage                                                            | Free (when using the shared slot pool)            |
| Streaming Reads            | Use the storage Read API to perform streaming reads of table data.                            | Starting at $1.10 per TiB read                    |


O Google BigQuery é um Data Warehouse totalmente gerenciado e altamente escalável que permite executar consultas SQL super rápidas em grandes conjuntos de dados, sem a necessidade de gerenciar a infraestrutura.

A arquitetura sem servidor (serverless) do BigQuery permite usar consultas SQL para responder às perguntas de negócio sem precisar gerenciar a infraestrutura (configurar servidor e sistema operacional, por exemplo). O mecanismo de análise distribuída e escalonável do BigQuery permite consultar terabytes em segundos e petabytes em minutos.

O BigQuery maximiza a flexibilidade separando o mecanismo de computação que analisa os dados das suas opções de armazenamento. Você pode armazenar e analisar seus dados no BigQuery ou usar o BigQuery para avaliar seus dados onde eles estão. As consultas federadas permitem que você leia dados de fontes externas enquanto o streaming é compatível com atualizações contínuas de dados.

https://www.oreilly.com/library/view/google-bigquery-the/9781492044451/ch01.html

BigQuery, MapReduce and data ware house are fundamentally different technologies and each has different use cases:

<img width="476" alt="image" src="https://github.com/user-attachments/assets/db876e6f-6661-4674-82ea-b0c9ded8a188">

BigQuery is a fully managed, AI-ready data platform that helps you manage and analyze your data with built-in features like machine learning, search, geospatial analysis, and business intelligence. BigQuery's serverless architecture lets you use languages like SQL and Python to answer your organization's biggest questions with zero infrastructure management.

BigQuery provides a uniform way to work with both structured and unstructured data and supports open table formats like Apache Iceberg, Delta, and Hudi. BigQuery streaming supports continuous data ingestion and analysis while BigQuery's scalable, distributed analysis engine lets you query terabytes in seconds and petabytes in minutes.

BigQuery's architecture consists of two parts: a storage layer that ingests, stores, and optimizes data and a compute layer that provides analytics capabilities. These compute and storage layers efficiently operate independently of each other thanks to Google's petabit-scale network that enables the necessary communication between them.

Legacy databases usually have to share resources between read and write operations and analytical operations. This can result in resource conflicts and can slow queries while data is written to or read from storage. Shared resource pools can become further strained when resources are required for database management tasks such as assigning or revoking permissions. BigQuery's separation of compute and storage layers lets each layer dynamically allocate resources without impacting the performance or availability of the other.

This separation principle lets BigQuery innovate faster because storage and compute improvements can be deployed independently, without downtime or negative impact on system performance. It is also essential to offering a fully managed serverless data warehouse in which the BigQuery engineering team handles updates and maintenance. The result is that you don't need to provision or manually scale resources, leaving you free to focus on delivering value instead of traditional database management tasks.

BigQuery interfaces include Google Cloud console interface and the BigQuery command-line tool. Developers and data scientists can use client libraries with familiar programming including Python, Java, JavaScript, and Go, as well as BigQuery's REST API and RPC API to transform and manage data. ODBC and JDBC drivers provide interaction with existing applications including third-party tools and utilities.

As a data analyst, data engineer, data warehouse administrator, or data scientist, BigQuery helps you load, process, and analyze data to inform critical business decisions.

