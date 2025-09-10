# AWS Lambda (Computação Serverless)

O AWS Lambda é o coração da computação serverless na AWS. É um serviço de computação que permite executar código sem provisionar ou gerenciar servidores. Você paga apenas pelo tempo de computação que consome, e não há cobrança quando seu código não está em execução.

## Conceito Principal: Arquitetura Orientada a Eventos

O Lambda funciona com base em eventos. Seu código (a "função Lambda") fica inativo até que seja acionado por um evento.

-   **Evento (Event):** É o que inicia a execução da sua função. Pode ser:
    -   Uma requisição HTTP para o Amazon API Gateway.
    -   O upload de um novo objeto em um bucket S3.
    -   Uma nova mensagem em uma fila SQS ou tópico SNS.
    -   Uma alteração em uma tabela do DynamoDB.
    -   Uma programação agendada (ex: a cada 5 minutos) via Amazon EventBridge.
    -   Uma chamada direta pela AWS SDK.

-   **Função Lambda (Lambda Function):** É o seu código. O Lambda suporta várias linguagens, como Node.js, Python, Java, Go, C#, Ruby e PowerShell. Você faz o upload do seu código em um pacote de implantação (arquivo .zip).

-   **Execução:** Quando um evento ocorre, o Lambda automaticamente provisiona um ambiente de execução, executa sua função, e depois o desativa. Ele cuida de todo o escalonamento; se 1000 eventos chegam ao mesmo tempo, o Lambda tentará executar 1000 instâncias da sua função em paralelo.

## Principais Características e Configurações

-   **Configuração de Memória:** Você aloca uma quantidade de memória para sua função (de 128 MB a 10 GB). A quantidade de CPU e outros recursos é alocada proporcionalmente à memória. Mais memória = mais poder de CPU.

-   **Timeout:** É o tempo máximo que sua função pode executar, com um limite de **15 minutos**. Se a função exceder esse tempo, ela é terminada. Isso é crucial para evitar execuções descontroladas e custos inesperados.

-   **Modelo de Preços:** Você paga por:
    1.  **Número de Requisições:** O número de vezes que sua função é acionada.
    2.  **Duração da Execução:** O tempo que sua função leva para executar, medido em milissegundos e ponderado pela memória alocada.

-   **Funções Síncronas vs. Assíncronas:**
    -   **Invocação Síncrona:** O serviço que invoca a função espera pela resposta. Ex: Uma chamada do API Gateway espera o resultado da função para retornar ao cliente.
    -   **Invocação Assíncrona:** O serviço que invoca a função apenas entrega o evento ao Lambda e não espera por uma resposta. Ex: Um evento do S3. Para invocações assíncronas, o Lambda tenta executar a função novamente em caso de falha.

-   **Controle de Concorrência:**
    -   **Concurrency (Concorrência):** É o número de execuções que sua função está servindo a qualquer momento. Por padrão, há um limite de 1000 execuções concorrentes por conta por região (pode ser aumentado).
    -   **Reserved Concurrency (Concorrência Reservada):** Você pode reservar uma quantidade de concorrência para uma função específica, garantindo que ela sempre terá capacidade para executar, sem competir com outras funções.
    -   **Provisioned Concurrency (Concorrência Provisionada):** Mantém um número de ambientes de execução "quentes" (inicializados e prontos para executar), eliminando a latência de "cold start" para funções sensíveis à latência. Isso tem um custo adicional.

-   **Lambda e Redes (VPC):**
    -   Por padrão, as funções Lambda executam em uma VPC gerenciada pela AWS e podem acessar a internet e outros serviços públicos da AWS.
    -   Se sua função precisa acessar recursos dentro da sua própria VPC (como um banco de dados RDS), você deve configurá-la para se conectar à sua VPC. Ao fazer isso, a função perde o acesso direto à internet. Para permitir que ela acesse tanto os recursos da VPC quanto a internet, você precisa rotear seu tráfego de saída através de um **NAT Gateway** na sua VPC.

## Casos de Uso Típicos

-   **Backend para Aplicações Web:** Usar API Gateway + Lambda para criar APIs RESTful totalmente serverless.
-   **Processamento de Dados em Tempo Real:** Processar e transformar dados à medida que chegam (ex: redimensionar imagens enviadas para o S3).
-   **Automação de TI:** Executar tarefas agendadas, como fazer backups, verificar conformidade de recursos ou limpar recursos não utilizados.
-   **Chatbots e Backends para Alexa Skills.**
