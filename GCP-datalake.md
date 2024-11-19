To prevent getting billed for usage beyond the free courtesy usage limits, you can set requests per day caps: https://cloud.google.com/apis/docs/capping-api-usage

# Colossus (Google File System)

https://cloud.google.com/blog/products/storage-data-transfer/a-peek-behind-colossus-googles-file-system

All of Google is layered with a common set of scalable services. There are three main building blocks used by each of our storage services:

+ Colossus is our cluster-level file system, successor to the Google File System (GFS).
+ Spanner is our globally-consistent, scalable relational database.
+ Borg is a scalable job scheduler that launches everything from compute to storage services. It was and continues to be a big influence on the design and development of Kubernetes.

These three core building blocks are used to provide the underlying infrastructure for all Google Cloud storage services, from Firestore to Cloud SQL to Filestore, and Cloud Storage

Borg provisions the needed resources, Spanner stores all the metadata about access permissions and data location, and then Colossus manages, stores, and provides access to all your data. 

Colossus characteristics:

+ It’s the next-generation of the GFS.
+ Its design enhances storage scalability and improves availability to handle the massive growth in data needs of an ever-growing number of applications.
+ Colossus introduced a distributed metadata model that delivered a more scalable and highly available metadata subsystem.  

Colossus control plane:

<img width="619" alt="image" src="https://github.com/user-attachments/assets/9c42efe0-9aeb-4269-808c-2f9672b3b194">

+ Client library
  + The client library is how an application or service interacts with Colossus.
  + The client is probably the most complex part of the entire file system.
  + There’s a lot of functionality, such as software RAID, that goes into the client based on an application’s requirements.
  + Applications built on top of Colossus use a variety of encodings to fine-tune performance and cost trade-offs for different workloads.
+ Colossus Control Plane
  + The foundation of Colossus is its scalable metadata service, which consists of many Curators.
  + Clients talk directly to curators for control operations, such as file creation, and can scale horizontally.
+ Metadata database
  + Curators store file system metadata in Google’s high-performance NoSQL database, BigTable.
  + The original motivation for building Colossus was to solve scaling limits we experienced with Google File System (GFS) when trying to accommodate metadata related to Search.
  + Storing file metadata in BigTable allowed Colossus to scale up by over 100x over the largest GFS clusters.
+ D File Servers
  + Colossus also minimizes the number of hops for data on the network.
  + Data flows directly between clients and “D” file servers (our network attached disks).
+ Custodians
  + Colossus also includes background storage managers called Custodians.
  + They play a key role in maintaining the durability and availability of data as well as overall efficiency, handling tasks like disk space balancing and RAID reconstruction. 

<img width="642" alt="image" src="https://github.com/user-attachments/assets/09c78d3a-95b8-4a62-830b-1bde0b3e8505">

+ With Colossus, a single cluster is scalable to exabytes of storage and tens of thousands of machines.
+ In the example above, for example, we have instances accessing Cloud Storage from Compute Engine VMs, YouTube serving nodes, and Ads MapReduce nodes—all of which are able to share the same underlying file system to complete requests.
+ The key ingredient is having a shared storage pool that is managed by the Colossus control plane, providing the illusion that each has its own isolated file system. 

# BigQuery

O BigQuery é um data warehouse corporativo totalmente gerenciado que ajuda a gerenciar e analisar dados com recursos integrados, como aprendizado de máquina, análise geoespacial e business intelligence. 

+ A arquitetura sem servidor do BigQuery permite usar consultas SQL para responder às maiores questões de uma organização sem a necessidade de gerenciamento de infraestrutura.
+ As consultas federadas permitem que você leia dados de fontes externas enquanto o streaming é compatível com atualizações contínuas de dados.
+ O mecanismo de análise distribuída e escalonável do BigQuery permite consultar terabytes em segundos e petabytes em minutos.

## Storage 

A arquitetura do BigQuery consiste em duas partes: uma camada de armazenamento que ingere, armazena e otimiza dados e uma camada de computação que fornece recursos de análise:

<img width="508" alt="image" src="https://github.com/user-attachments/assets/19ea8eb5-18cc-4e07-a529-fc007bf18b0c">

### Dados da tabela

A maioria dos dados armazenados no BigQuery são dados de tabelas. Os dados incluem tabelas padrão, clones de tabelas, instantâneos de tabelas e visualizações materializadas. Você é cobrado pelo armazenamento que usar com esses recursos. 

+ Quando você executa uma consulta, o mecanismo de consulta distribui o trabalho em paralelo para vários workers, que verificam as tabelas relevantes no armazenamento, processam a consulta e coletam os resultados.
+ O BigQuery executa consultas totalmente na memória, usando uma rede de petabits para garantir que os dados se movam com muita rapidez até os nós de trabalho.
+ Você é cobrado pelo armazenamento que usar com esses recursos
  + As tabelas padrão contêm dados estruturados.
    + Todas as tabelas têm um esquema, e cada coluna tem um tipo de dados.
    + O BigQuery armazena dados em formato de colunas.
  + Os clones de tabelas são cópias leves e graváveis de tabelas padrão.
    + O BigQuery só armazena o delta entre um clone de tabela e a tabela base.
  + Os snapshots de tabelas são cópias pontuais de tabelas.
    + Os snapshots de tabelas são somente leitura, mas é possível restaurar uma tabela a partir de um snapshot de tabela.
    + O BigQuery armazena somente o delta entre um snapshot de tabela e a tabela base.

Além disso, os resultados da consulta armazenados em cache são mantidos como tabelas temporárias. Não há cobrança pelos resultados de consulta armazenados em cache armazenados em tabelas temporárias.

As visualizações materializadas são visualizações pré-computadas que armazenam em cache os resultados da consulta da visualização periodicamente. Os resultados armazenados em cache são mantidos no armazenamento do BigQuery.

### Metadados

O armazenamento do BigQuery também contém metadados sobre os recursos do BigQuery. O armazenamento de metadados não é cobrado.

+ Quando você cria uma entidade permanente no BigQuery, como uma função de tabela, visualização ou definida pelo usuário (UDF, na sigla em inglês), o BigQuery armazena metadados sobre a entidade.
  + Isso é válido mesmo para recursos que não contêm dados de tabelas, como UDFs e visualizações lógicas.
+ Os metadados incluem informações como esquema de tabela, especificações de particionamento e clustering, prazos de validade da tabela, entre outras.
  + Esse tipo de metadados é visível para o usuário e pode ser configurado quando você cria o recurso.
  + Além disso, o BigQuery armazena os metadados usados internamente para otimizar as consultas.
  + Esses metadados não ficam diretamente visíveis aos usuários.

### Layout do armazenamento

O BigQuery armazena dados de tabelas em formato de colunas, isto é, ele armazena cada coluna separadamente.
+ Os bancos de dados orientados por coluna são especialmente eficientes na verificação de colunas individuais em um conjunto de dados inteiro.
+ Os bancos de dados orientados por coluna são otimizados para cargas de trabalho analíticas que agregam dados sobre um número muito grande de registros.
+ Muitas vezes, uma consulta analítica precisa apenas ler algumas colunas de uma tabela.
+ Por exemplo, se você quiser calcular a soma de uma coluna em milhões de linhas, o BigQuery pode ler os dados dessa coluna sem ler todos os campos de todas as linhas.
+ Outra vantagem dos bancos de dados orientados por coluna é que os dados em uma coluna geralmente têm mais redundância do que os dados em uma linha.
+ Essa característica permite uma maior compactação de dados usando técnicas como a codificação de tamanho de execução, que pode melhorar o desempenho da leitura.

### Modelos de faturamento do Storage

É possível receber cobranças pelo armazenamento de dados do BigQuery em bytes lógicos ou físicos (compactados) ou uma combinação de ambos.

Você define o modelo de faturamento do armazenamento no nível do conjunto de dados. Se você não especificar um modelo de faturamento do armazenamento ao criar um conjunto de dados, o padrão será usar o faturamento do armazenamento lógico.

### Otimizar o armazenamento

A otimização do armazenamento do BigQuery melhora o desempenho das consultas e controla os custos. Para acessar os metadados de armazenamento de tabelas, consulte as seguintes visualizações INFORMATION_SCHEMA:

INFORMATION_SCHEMA.TABLE_STORAGE
INFORMATION_SCHEMA.TABLE_STORAGE_BY_ORGANIZATION

### Carregar dados

Padrões básicos de ingestão de dados no BigQuery.

+ Carregamento em lote: carregue os dados de origem em uma tabela do BigQuery em uma única operação em lote.
  + Essa operação pode ser única ou pode ser automatizada para ocorrer de acordo com uma programação.
  + Uma operação de carregamento em lote pode criar uma nova tabela ou anexar dados a uma tabela existente.
+ Streaming: faça streaming contínuo de lotes menores de dados, para que os dados fiquem disponíveis para consultas quase em tempo real.
+ Dados gerados: use instruções SQL para inserir linhas em uma tabela atual ou gravar os resultados de uma consulta em uma tabela.

### Ler dados do armazenamento do BigQuery

O BigQuery oferece várias maneiras de ler os dados da tabela:

+ API BigQuery: acesso síncrono paginado com o método tabledata.list.
  + Os dados são lidos em série, uma página por invocação. 
+ API BigQuery Storage: acesso de alta capacidade de streaming que também oferece suporte à projeção e filtragem de colunas do lado do servidor.
  + As leituras podem ser carregadas em paralelo por vários leitores se forem segmentadas em vários streams separados.
+ Exportar: cópia assíncrona de alta capacidade de processamento para o Google Cloud Storage, seja com jobs de extração ou EXPORT DATA.
  + Se você precisar copiar dados no Cloud Storage, exporte-os com um job de extração ou uma instrução EXPORT DATA.
+ Cópia: cópia assíncrona de conjuntos de dados no BigQuery.
  + A cópia é feita de maneira lógica quando os locais de origem e de destino são iguais.

### Exclusão

Quando você exclui uma tabela, os dados persistem por pelo menos a duração da janela de viagem no tempo. Depois disso, os dados são limpos do disco dentro da linha do tempo de exclusão do Google Cloud. Algumas operações de exclusão, como a instrução DROP COLUMN, são apenas operações de metadados. Nesse caso, o armazenamento será liberado na próxima vez que você modificar as linhas afetadas. Se você não modificar a tabela, não haverá um tempo garantido dentro do qual o armazenamento será liberado.

Os bancos de dados legados geralmente precisam compartilhar recursos para operações de leitura/gravação e analíticas. Isso pode resultar em conflitos de recursos e tornar as consultas mais lentas enquanto os dados são gravados ou lidos a partir do armazenamento. Os pools de recursos compartilhados podem ficar ainda mais sobrecarregados quando são necessários recursos para tarefas de gerenciamento de banco de dados, como atribuição ou revogação de permissões. Com a separação das camadas de computação e armazenamento do BigQuery, cada uma delas pode alocar recursos dinamicamente sem afetar o desempenho ou a disponibilidade da outra.
https://www.oreilly.com/library/view/google-bigquery-the/9781492044451/ch01.html

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


### BigQuery under the hood

https://cloud.google.com/blog/products/bigquery/bigquery-under-the-hood

BigQuery employs a vast set of multi-tenant services driven by low-level Google infrastructure technologies:

<img width="396" alt="image" src="https://github.com/user-attachments/assets/bad3a60f-65f8-4f87-a986-faeaa87e0621">

+ Dremel: The Execution Engine
  + Dremel turns SQL queries into execution trees. The leaves of the tree are called slots and do the heavy lifting of reading data from storage and any necessary computation. The branches of the tree are ‘mixers’, which perform the aggregation.
  + Dremel dynamically apportions slots to queries on an as-needed basis, maintaining fairness for concurrent queries from multiple users. A single user can get thousands of slots to run their queries.
+ Colossus: Distributed Storage
  + BigQuery leverages the columnar storage format and compression algorithm to store data in Colossus, optimized for reading large amounts of structured data.
  + Colossus also handles replication, recovery (when disks crash) and distributed management (so there is no single point of failure). Colossus allows BigQuery users to scale to dozens of petabytes of data stored seamlessly, without paying the penalty of attaching much more expensive compute resources as in traditional data warehouses.
+ Jupiter: The Network
  + In between storage and compute is ‘shuffle’, which takes advantage of Google’s Jupiter network to move data extremely rapidly from one place to another.
+ Borg: Compute
  + The mixers and slots are all run by Borg, which allocates hardware resources.

In the end, these low level infrastructure components are combined with several dozen high-level technologies, APIs, and services — like Bigtable, Spanner, and Stubby — to make one transparent and powerful analytics database — BigQuery.

### Price

<img width="665" alt="image" src="https://github.com/user-attachments/assets/68378d75-71c4-4157-a591-57e0cc64d8aa">

# Google Cloud for Students

https://cloud.google.com/edu/students?hl=en

Create BigLake external tables for Cloud Storage:
https://cloud.google.com/bigquery/docs/create-cloud-storage-table-biglake#console

# Cloud Storage

Free Tier is only available in us-east1, us-west1, and us-central1 regions.

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

# Pub/Sub

O Pub/Sub é um serviço de mensagens assíncronas, com latências de 100 milissegundos. O Pub/Sub dissocia serviços que produzem eventos de serviços que processam eventos. É possível usar o Pub/Sub como middleware orientado a mensagens ou ingestão e entrega de eventos para pipelines de análise de streaming. Em ambos os cenários, um aplicativo de editor cria e envia mensagens para um tópico. Para receber essas mensagens, os aplicativos do assinante criam uma assinatura de um tópico. Uma assinatura é uma entidade nomeada que representa um interesse em receber mensagens sobre um tópico específico.

O Pub/Sub é executado em todas as regiões do Google Cloud. O Pub/Sub direciona o tráfego do editor para o data center do Google Cloud mais próximo em que é permitido o armazenamento de dados, conforme definido na política de restrição de local de recursos.

O Pub/Sub pode se integrar a muitos serviços do Google Cloud comofluxo ,Armazenamento em nuvem ,Cloud Run para criar um anexo da VLAN de monitoramento. É possível configurar esses serviços para servirem como origens de dados que podem publicar mensagens no Pub/Sub ou como coletores de dados que podem receber mensagens do Pub/Sub.

O Pub/Sub é usado para análises de streaming e pipelines de integração de dados para carregar e distribuir dados. É igualmente eficaz como um middleware voltado para mensagens para integração de serviços ou como uma fila para carregar tarefas em paralelo.

O Pub/Sub permite criar sistemas de produtores e consumidores de eventos, chamados editores e inscritos. Os editores se comunicam com os assinantes de forma assíncrona transmitindo eventos, em vez de realizar chamadas de procedimento remoto (RPCs) síncronas.

Os editores enviam eventos ao serviço Pub/Sub, sem considerar como ou quando esses eventos serão processados. Em seguida, o Pub/Sub envia eventos para todos os serviços que reagem a eles. Nos sistemas que se comunicam por RPCs, os editores precisam esperar os assinantes receberem os dados. No entanto, a integração assíncrona no Pub/Sub aumenta a flexibilidade e a robustez do sistema geral.

# Cloud Run functions

You bring the code, we handle the rest by making it simple to build and easy to maintain your platform.

Cloud Functions is now Cloud Run functions. You can write and deploy functions with Cloud Run, giving you complete control over the underlying service configuration.

https://cloud.google.com/functions#real-time-stream-processing

Use cases (relevant):
Real-time stream processing

Use Cloud Run functions to respond to events from Pub/Sub to process, transform, and enrich streaming data in transaction processing, click-stream analysis, application activity tracking, IoT device telemetry, social media analysis, and other types of applications.

<img width="564" alt="image" src="https://github.com/user-attachments/assets/b2c6adbc-819e-4967-8945-983a5958f5e3">

Real-time file processing

Execute your code in response to changes in data. Cloud Run functions can respond to events from Google Cloud services, such as Cloud Storage, Pub/Sub, and Cloud Firestore to process files immediately after upload and generate thumbnails from image uploads, process logs, validate content, transcode videos, validate, aggregate, and filter data in real time.

# Integração entre os serviços 

<img width="617" alt="image" src="https://github.com/user-attachments/assets/f93b40a3-27cb-49f5-8479-9e99de263039">
