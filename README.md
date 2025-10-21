# Implantação e Avaliação de Data Lakes em Free Tier Clouds

## Oracle Cloud Infrastructure (OCI) 

### Cluster

<p align="center"> <img width="758" alt="image" src="https://github.com/user-attachments/assets/9acbf88f-3e63-412e-9653-6d5d5a53f464">

### Stack

<p align="center"> <img width="660" alt="image" src="https://github.com/user-attachments/assets/84f3269b-2cfa-44a8-b739-eeb838d002e7">

Descrição das Ferramentas de Código Aberto:

- A biblioteca de software Apache Hadoop é um framework que permite o processamento distribuído de grandes conjuntos de dados através de clusters de computadores, usando modelos de programação simples. Ele é projetado para escalar de servidores únicos a milhares de máquinas, cada uma oferecendo computação e armazenamento local.

- O Hadoop Distributed File System (HDFS) é um sistema de arquivos distribuído projetado para rodar em hardware comum (commodity hardware). O HDFS é altamente tolerante a falhas e foi projetado para ser implantado em hardware de baixo custo. O HDFS fornece acesso de alta taxa de transferência aos dados de aplicativos e é adequado para aplicações que possuem grandes conjuntos de dados.

- O projeto Apache Ambari tem como objetivo simplificar o gerenciamento do Hadoop, desenvolvendo software para provisionar, gerenciar e monitorar clusters Apache Hadoop. O Ambari oferece uma interface de usuário web intuitiva e fácil de usar para o gerenciamento do Hadoop, suportada por suas APIs RESTful.

- Nifi é um sistema fácil de usar, poderoso e confiável para processar e distribuir dados.

- Apache Kafka é uma plataforma de streaming de eventos distribuída e de código aberto, usada por milhares de empresas para pipelines de dados de alto desempenho, análise de streaming, integração de dados e aplicações de missão crítica.

- ZooKeeper é um serviço centralizado para manter informações de configuração, nomeação, fornecer sincronização distribuída e oferecer serviços de grupo. Todos esses tipos de serviços são usados de alguma forma por aplicações distribuídas.

- Apache Hive é um sistema de data warehouse distribuído e tolerante a falhas que permite análises em escala massiva. O Hive Metastore (HMS) fornece um repositório central de metadados que podem ser facilmente analisados para tomar decisões informadas e baseadas em dados, e, portanto, é um componente crítico de muitas arquiteturas de data lake.

- Apache Spark™ é um motor de múltiplas linguagens para executar engenharia de dados, ciência de dados e aprendizado de máquina em máquinas de nó único ou em clusters.

## Google Cloud Platform (GCP)

<p align="center"> <img width="605" height="277" alt="image" src="https://github.com/user-attachments/assets/998d2d40-12fc-48f7-8b93-42a9cd00cc03" />

Descrição das ferramentas da nuvem da Google:

- O BigQuery é a plataforma autônoma de dados para IA que automatiza todo o ciclo de vida dos dados, desde a ingestão até os insights baseados em IA, para que você possa ir dos dados à IA e à ação mais rapidamente.
  - 10 GiB de armazenamento, até 1 TiB de consultas em computação sob demanda sem custos por mês e outros recursos
  - Importar tabela do Cloud Storage e exportar dados de tabelas para o Cloud Storage
- O Cloud Storage é um serviço gerenciado para armazenar dados não estruturados. Armazene qualquer quantidade de dados e recupere-os com a frequência que desejar.
  - 5 GB-meses de armazenamento regional (apenas regiões dos EUA) por mês
  - 5.000 Operações de Classe A por mês
  - 50.000 Operações de Classe B por mês
  - 100 GB de transferência de dados de saída da América do Norte para todos os destinos de região (excluindo China e Austrália) por mês
- O Pub/Sub é um serviço de mensagens assíncrono e escalonável que separa os serviços que produzem mensagens dos serviços que processam essas mensagens.
  - 10 GB de mensagens por mês
- O Cloud Run é uma plataforma totalmente gerenciada que permite executar seu código diretamente na infraestrutura escalonável do Google. O Cloud Run é simples, automatizado e projetado para aumentar sua produtividade.
  -  2 milhões de solicitações por mês
  -  360.000 GB-segundos de memória, 180.000 vCPU-segundos de tempo de computação
  -  1 GB de transferência de dados de saída da América do Norte por mês

## IBM Cloud

watsonx.data:

<p align="center"> <img width="689" height="320" alt="image" src="https://github.com/user-attachments/assets/0eb46da5-e009-4bf3-8477-621465b67291" />

- O IBM watsonx.data é um data lakehouse híbrido e aberto, construído para escalar cargas de trabalho de IA e análises com todos os seus dados, onde quer que eles estejam
  - Seu free tier oferece 2.000 unidades de recursos para testar o serviço, o que pode ser usado em até 30 dias
