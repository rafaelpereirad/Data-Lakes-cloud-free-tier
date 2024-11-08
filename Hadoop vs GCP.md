
# Processamento

|  | Hive | Spark | BigQuery |
|----------|----------|----------|----------|
| Função | Sistema de data warehouse distribuído e tolerante a falhas que permite análises em grande escala. | Estrutura de análise de dados que pode realizar análises complexas em memória em grandes volumes de dados de até petabytes. | Plataforma de análise de dados totalmente gerenciada e pronta para IA que ajuda a impulsionar o valor dos dados, funcionando com vários mecanismos, formatos e nuvens. |
| Tipo de processamento | Processamento em lote usando frameworks computacionais Apache Tez ou MapReduce. | Processamento na memória, reduzindo o número de etapas em uma tarefa e reutilizando dados em várias operações paralelas. | Processamento interativo e distribuído usando o serviço Dremel para consultas. |
| Latência | Média a alta, dependendo da capacidade de resposta do mecanismo de computação. O modelo de execução distribuída oferece performance superior em comparação com sistemas de consulta monolíticos, como o RDBMS, para os mesmos volumes de dados.  | O Spark SQL é um mecanismo de consulta distribuído que fornece consultas interativas de baixa latência até 100 vezes mais rápidas que o MapReduce |  |
| Tipos de dados | Oferece suporte a dados estruturados e não estruturados. Fornece suporte nativo para tipos de dados SQL comuns, como INT, FLOAT e VARCHAR.  | 
| Tipo | Data Warehouse | Framework para Análise de dados | Data Warehouse |
| Formato armazenamento | CSV, TSV, Sequencial, RCFile, Avro, ORC e Parquet | CSV, JSON, ORC, e Parquet | Colunar |
| Linguagem de Consulta | HiveQL | SQL, Java, Scala, Python e R | SQL |
| Arquitetura | Hive Server 2 (aceita solicitações de usuários e aplicativos e cria planos de execução), Hive Query Language (HQL), Apache Hive Metastore externo (armazena todos os metadados do Hive) e Hive Beeline Shell. | Driver Process (mantem as informações sobre o aplicativo Spark, responde à consulta do usuário e agenda e distribui tarefas aos executores), Cluster Manager e Executors (armazenamento dos dados e execução do código nos dados distribuídos) | Uma camada de armazenamento que ingere, armazena e otimiza dados e uma camada de computação que fornece recursos de análise |

|  | YARN e Mapreduce | Dremel |
|----------|----------|----------|
|  |  YARN is to split up the functionalities of resource management and job scheduling/monitoring into separate daemons. | High-level, SQL-like language to express ad hoc queries | 
|  | MapReduce is a software framework for easily writing applications which process vast amounts of data (multi-terabyte data-sets) in-parallel on large clusters | Executes queries natively without translating them into MR jobs | 
|  |  | Storage and reduce CPU cost due to cheaper compression | 
|  |  | Inspired Apache Drill (open source) | 

https://siliconangle.com/2012/08/20/googles-dremel-here-comes-a-new-challenger-to-yarnhadoop/

https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html

# Armazenamento 

|  | HDFS | Colossus (sucessor to GFS) |
|----------|----------|----------|
| Implementation | In the other part, HDFS based on Apache Hadoop open-source project can be deployed and used by any company willing to manage and process big data. | Since GFS is proprietary file system and exclusive to Google only, it can not be used by any other company. |
| File serving | In Hadoop, HDFS file system divides the files into units called blocks of 128 MB in size. Block size can be adjustable based on the size of data. | In GFS, files are divided into units called chunks of fixed size. Chunk size is 64 MB and can be stored on different nodes in cluster for load balancing and performance needs. |
| Cache management | The HDFS has “DistributedCache”. DistributedCache is facility provided by the MapReduce to distribute application-specific, large, read-only files efficiently. It also caches files such as text, archives (zip, tar, tgz and tar.gz) and jars needed by applications. DistributedCache files can be private or public, that determines how they can be shared on the slave nodes. |  In GFS, cache metadata are saved in client memory. Chunk server does not need cache file data. Linux system running on the chunk server caches frequently accessed data in memory |
| Files protection and permission | The HDFS implements POSIX-like mode permission for files and directories. All files and directories are associated with an owner and a group with separate permissions for users who are owners, for users that are members of the group and for all other users. | Suitebriar-Google partner-mentions in its security analysis research that GFS splits files up and stores it in multiple pieces on multiple machines. File names have random names and are not human readable. Files are obfuscated through algorithms that change constantly. |
| Replication strategy | The HDFS has an automatic replication rack based system. By default two copies of each block are stored by different DataNodes in the same rack and a third  opy is stored on a DataNode on a different rack. | The GFS has two replicas: Primary replicas and secondary replicas. A primary replica is the data chunk that a chunk server sends to a client. Secondary replicas serve as backups on other chunk servers. User can specify the number of replicas to be maintained. |
| File namespace | The HDFS supports a traditional hierarchical file organization. Users or application can create directories to store files inside. The HDFS also supports third-party file systems such as CloudStore and Amazon Simple Storage Service (S3) | In GFS, files are organized hierarchically in directories and identified by path names. The GFS is exclusively for Google only |
| Filesystem database | Based on "Bigtable" study white papers, Apache developed its own database called HBase in Hadoop open-source project. The HBase is built with Java language. The major common features between bigtable and HBase. | The GFS has bigtable database. Bigtable is a proprietary database developed by Google using c++. |


Scalability: Both HDFS and GFS are considered as cluster based architecture. Each file system runs over machines built from commodity hardware. Each cluster may consist of thousands of nodes with huge data size storage.


While GFS was groundbreaking in its own right, it had limitations in terms of scalability. To address these issues, Google developed Colossus as an improved version of GFS. Colossus provides storage for various Google products and serves as the underlying storage platform for Google Cloud services, making it publicly available. With enhanced scalability and availability, Colossus is designed to handle modern applications' rapidly growing data demands.

Google File System architecture:

<img width="568" alt="image" src="https://github.com/user-attachments/assets/c416dce0-15cf-4887-8091-1543b75354f3">

https://www.infoq.com/articles/dfs-architecture-comparison/
Research Article: Analyzing Google File System and Hadoop Distributed File System, Nader Gemayel

# Ingestão

|  | Kafka | Pub/Sub |
|----------|----------|----------|
| Ordem das mensagens	| Em partições | Dentro de tópicos |
| Gerenciamento de infraestrutura	 | Implementar e operar manualmente máquinas virtuais (VMs). Mantenha versões e patches consistentes.	 | Gerenciado pelo Google |
| Limite mensagens | Configurável | 10 MB |

<img width="539" alt="image" src="https://github.com/user-attachments/assets/f76897b7-d07b-4603-8a46-194637d73e5a">

+ No diagrama anterior, cada M representa uma mensagem.
+ Os agentes do Kafka gerenciam várias partições ordenadas de mensagens, representadas pelas linhas horizontais de mensagens.
+ Os consumidores leem mensagens de uma partição específica que tem capacidade com base na máquina que hospeda essa partição.
+ O Pub/Sub não tem partições, e os consumidores leem de um tópico que realiza o escalonamento automático de acordo com a demanda.
+ Você configura cada tópico do Kafka com o número de partições necessárias para processar a carga de consumidor esperada.
+ O Pub/Sub é escalonado automaticamente com base na demanda.

https://aws.amazon.com/pt/what-is/apache-hive/

https://logz.io/blog/hive-vs-spark/

https://cloud.google.com/bigquery?hl=pt

https://cwiki.apache.org/confluence/display/Hive/FileFormats

https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36632.pdf

https://www.databricks.com/br/glossary/apache-hive

https://spark.apache.org/

https://blog.dsacademy.com.br/serie-spark-e-databricks-parte-1-arquitetura-e-componentes-do-apache-spark/

https://cloud.google.com/pubsub/docs/migrating-from-kafka-to-pubsub?hl=pt-br
