### O que é Amazon Kinesis

O Amazon Kinesis é uma plataforma para dados de streaming na AWS, que facilita o carregamento e a análise de dados de streaming em tempo real. Ele permite que você processe e analise dados à medida que chegam, em vez de ter que esperar até que todos os dados sejam coletados antes que o processamento possa começar. É ideal para casos de uso como análise de logs e eventos, processamento de feeds de cliques de sites e análise de dados de IoT.

A família Kinesis é composta por quatro serviços principais:

---

### 1. Kinesis Data Streams

*   **O que é:** Um serviço de ingestão e processamento de dados de streaming em tempo real, massivamente escalável e durável.
*   **Como funciona:**
    *   **Produtores (Producers):** Aplicações (servidores, clientes, dispositivos IoT) enviam registros de dados para um Kinesis Data Stream.
    *   **Stream:** O stream é composto por uma ou mais **shards (fragmentos)**. Um shard é uma unidade de throughput (1 MB/s de entrada, 2 MB/s de saída). Você adiciona shards para aumentar a capacidade do stream.
    *   **Consumidores (Consumers):** Aplicações que leem e processam os registros do stream. Vários consumidores podem ler do mesmo stream. Os dados são mantidos no stream por um período de retenção (padrão de 24 horas, até 365 dias).
*   **Casos de uso:** Análise de logs em tempo real, análise de clickstream, processamento de dados de dispositivos IoT.

---

### 2. Kinesis Data Firehose

*   **O que é:** A maneira mais fácil de carregar dados de streaming de forma confiável na AWS. Ele captura, transforma e carrega dados de streaming em data lakes, data warehouses e serviços de análise.
*   **Como funciona:**
    *   **Captura:** O Firehose captura dados de streaming de fontes como Kinesis Data Streams, CloudWatch Logs ou diretamente de produtores.
    *   **Transformação (Opcional):** Ele pode transformar os dados em lote usando uma função do AWS Lambda (por exemplo, converter de JSON para Parquet) antes de carregá-los.
    *   **Carregamento:** Ele carrega os dados em destinos como **Amazon S3**, **Amazon Redshift**, **Amazon OpenSearch Service (Elasticsearch)** e **Splunk**.
*   **Diferença para o Data Streams:** O Firehose é um serviço de entrega totalmente gerenciado. Você não precisa gerenciar shards ou consumidores. Ele é projetado para o padrão "carregar para o destino" (load to destination). O Data Streams é mais flexível e projetado para processamento personalizado em tempo real com consumidores.

---

### 3. Kinesis Data Analytics

*   **O que é:** A maneira mais fácil de analisar dados de streaming, obter insights acionáveis e responder a eles em tempo real.
*   **Como funciona:**
    *   **Entrada:** Ele lê dados de um Kinesis Data Stream ou Kinesis Data Firehose.
    *   **Processamento:** Você escreve código de análise de dados usando **SQL** (para aplicações mais simples) ou **Apache Flink** (para aplicações mais complexas em Java/Scala). O serviço gerencia a infraestrutura para executar suas consultas continuamente sobre os dados de streaming.
    *   **Saída:** Os resultados da análise podem ser enviados para outro Kinesis Data Stream, Kinesis Data Firehose ou uma função Lambda.
*   **Casos de uso:** Geração de métricas em tempo real, análise de séries temporais, detecção de anomalias em tempo real.

---

### 4. Kinesis Video Streams

*   **O que é:** Facilita o streaming seguro de vídeo de dispositivos conectados para a AWS para análise, machine learning (ML), reprodução e outros processamentos.
*   **Como funciona:** Ele provisiona e escala elasticamente toda a infraestrutura necessária para ingerir dados de streaming de vídeo de milhões de dispositivos. Ele armazena, criptografa e indexa os dados de vídeo de forma durável em seus streams e permite que você acesse seus dados por meio de APIs fáceis de usar.
*   **Casos de uso:** Streaming de câmeras de segurança, babás eletrônicas, análise de vídeo com ML.

### Benefícios do Kinesis

*   **Processamento em Tempo Real:** Permite que você obtenha insights de seus dados em segundos ou minutos, em vez de horas ou dias.
*   **Totalmente Gerenciado:** A AWS gerencia a infraestrutura, permitindo que você se concentre na sua aplicação, não na operação do cluster de streaming.
*   **Escalabilidade:** Projetado para lidar com qualquer quantidade de dados de streaming, de megabytes a terabytes por hora.
