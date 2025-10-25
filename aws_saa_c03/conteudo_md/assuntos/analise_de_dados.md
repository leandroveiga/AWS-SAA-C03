# Análise de Dados na AWS

**Serviços Cobertos:** Kinesis (Data Streams, Firehose, Data Analytics), Athena, Glue (Data Catalog, Jobs, DataBrew), OpenSearch Service, QuickSight.

A AWS oferece um conjunto poderoso de serviços para coletar, processar, analisar e visualizar dados em tempo real e em lote. Esses serviços são fundamentais para a construção de data lakes, pipelines de análise e aplicações orientadas a dados.

---

## 1. Amazon Kinesis

O Amazon Kinesis é uma plataforma para coletar, processar e analisar dados de streaming em tempo real.

-   **Principais Casos de Uso:** Análise de logs e eventos em tempo real, processamento de dados de cliques em sites, análise de dados de IoT.

### Componentes do Kinesis:
-   **Kinesis Data Streams:** Captura e armazena fluxos de dados para processamento personalizado. Os dados são retidos por um período (24 horas por padrão, até 365 dias) para permitir que múltiplos consumidores os processem.
-   **Kinesis Data Firehose:** A maneira mais fácil de carregar dados de streaming em data lakes, data stores e ferramentas de análise. Ele captura, transforma e carrega dados de streaming para destinos como S3, Redshift, OpenSearch e Splunk, sem a necessidade de escrever código de aplicação.
-   **Kinesis Data Analytics:** Permite processar e analisar dados de streaming usando SQL padrão ou Apache Flink.

---

## 2. Amazon Athena

O Amazon Athena é um serviço de consulta interativo que facilita a análise de dados diretamente no Amazon S3 usando SQL padrão.

-   **Como funciona:** É "serverless", então não há infraestrutura para gerenciar. Você simplesmente aponta o Athena para seus dados no S3, define o esquema e começa a consultar usando SQL.
-   **Principais Características:**
    -   **Pague por Consulta:** Você paga apenas pelas consultas que executa.
    -   **Baseado em Presto/Trino:** Usa um mecanismo de consulta distribuído de código aberto.
    -   **Integração com AWS Glue Data Catalog:** Usa o catálogo de metadados do Glue para armazenar informações sobre as tabelas e esquemas dos seus dados no S3.
-   **Caso de Uso:** Análise rápida e ad-hoc de arquivos de log, dados de transações ou qualquer outro conjunto de dados armazenado no S3, sem a necessidade de carregar os dados em um banco de dados.

---

## 3. AWS Glue

O AWS Glue é um serviço de extração, transformação e carga (ETL) totalmente gerenciado que facilita a preparação e o carregamento de dados para análise. Ele é "serverless", o que significa que não há infraestrutura para gerenciar.

-   **Principais Componentes:**
    -   **Data Catalog:** É um repositório de metadados centralizado. O Glue pode usar "crawlers" para descobrir automaticamente seus dados em serviços como Amazon S3, Amazon RDS e DynamoDB, inferir seus esquemas e popular o Data Catalog. Este catálogo pode então ser usado por outros serviços como Amazon Athena, Amazon Redshift Spectrum e Amazon EMR.
    -   **Jobs ETL:** O Glue pode gerar automaticamente scripts Python ou Scala (usando Apache Spark) para realizar transformações complexas nos seus dados.
    -   **Glue DataBrew:** Uma ferramenta de preparação de dados visual que permite limpar e normalizar dados sem escrever código.
-   **Casos de Uso:**
    -   Construir e gerenciar um data lake no S3.
    -   Executar pipelines de ETL para transformar dados brutos em formatos prontos para análise.
    -   Descoberta de Dados: Usar crawlers para manter um catálogo atualizado de todos os seus ativos de dados.

---

## 4. Amazon OpenSearch Service (anteriormente Elasticsearch Service)

O Amazon OpenSearch Service é um serviço gerenciado que facilita a implantação, operação e escalabilidade de clusters OpenSearch (ou Elasticsearch legado) na nuvem AWS. O OpenSearch é um conjunto de busca e análise de código aberto, derivado do Elasticsearch.

-   **Principais Casos de Uso:**
    -   **Análise de Logs e Monitoramento:** É um dos casos de uso mais populares. Ingerir, analisar e visualizar grandes volumes de dados de log de aplicações e infraestrutura em tempo real para monitorar a saúde e a performance dos sistemas.
    -   **Busca em Texto Completo (Full-Text Search):** Potencializar a funcionalidade de busca em um site ou aplicação, oferecendo resultados rápidos e relevantes.
    -   **Visualização de Dados:** Integra-se com o OpenSearch Dashboards (ou Kibana) para criar painéis interativos e visualizações.
-   **Benefícios do Serviço Gerenciado:** A AWS gerencia tarefas complexas como provisionamento de hardware, instalação de software, patching, recuperação de falhas, backups e monitoramento, permitindo que você se concentre na utilização do serviço.

---

## 5. Amazon QuickSight

O Amazon QuickSight é um serviço de Business Intelligence (BI) escalável, serverless e incorporável, desenvolvido para a nuvem.

-   **Principais Casos de Uso:**
    -   **Análise de Negócios:** Criar painéis para acompanhar KPIs (Key Performance Indicators), métricas de vendas e performance operacional.
    -   **Visualização de Dados:** Transformar grandes volumes de dados brutos (ex: de logs do CloudTrail, dados de custos e uso da AWS, ou dados de aplicações) em gráficos e tabelas fáceis de entender.
    -   **BI Embarcado (Embedded Analytics):** Incorporar dashboards do QuickSight diretamente em suas aplicações, portais e websites.
-   **Motor de Análise (SPICE):** O QuickSight utiliza o SPICE (Super-fast, Parallel, In-memory Calculation Engine), um motor de cálculo em memória que otimiza as consultas para uma performance rápida.
