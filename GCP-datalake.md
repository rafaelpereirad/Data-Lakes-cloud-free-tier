To prevent getting billed for usage beyond the free courtesy usage limits, you can set requests per day caps: https://cloud.google.com/apis/docs/capping-api-usage

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


# Google Cloud for Students

https://cloud.google.com/edu/students?hl=en

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

# Pub/Sub

O Pub/Sub é um serviço de mensagens assíncrono e escalonável que separa os serviços que produzem mensagens dos serviços que as processam.

O Pub/Sub permite que os serviços se comuniquem de maneira assíncrona, com latências de 100 milissegundos.

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
