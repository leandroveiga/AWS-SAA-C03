# Serviços de Aplicação na AWS

A AWS oferece um conjunto de serviços que facilitam a criação de aplicações desacopladas, escaláveis e resilientes. Esses serviços são a espinha dorsal de muitas arquiteturas de microserviços.

## 1. Amazon SQS (Simple Queue Service)

O SQS é um serviço de enfileiramento de mensagens totalmente gerenciado que permite desacoplar e escalar microserviços, sistemas distribuídos e aplicações serverless.

-   **Conceito Principal:** Um componente (produtor) envia mensagens para uma fila, e outro componente (consumidor) processa essas mensagens de forma independente e em seu próprio ritmo. Isso desacopla os componentes; o produtor não precisa esperar pelo consumidor.
-   **Tipos de Fila:**
    -   **Fila Padrão (Standard):** Oferece taxa de transferência máxima, melhor esforço na ordenação (as mensagens podem ser entregues fora de ordem) e garantia de entrega de *pelo menos uma vez* (uma mensagem pode, raramente, ser entregue mais de uma vez). Ideal para a maioria dos casos de uso.
    -   **Fila FIFO (First-In, First-Out):** Garante que as mensagens sejam processadas exatamente na ordem em que são enviadas e a entrega é feita *exatamente uma vez*. A taxa de transferência é mais limitada. Ideal para cenários que exigem ordem estrita, como comandos para um sistema financeiro.
-   **Visibilidade do Timeout (Visibility Timeout):** Quando um consumidor lê uma mensagem, ela se torna invisível na fila por um período. Se o consumidor processar e deletar a mensagem, tudo certo. Se ele falhar e o timeout expirar, a mensagem se torna visível novamente para que outro consumidor possa processá-la.
-   **Dead-Letter Queue (DLQ):** Uma fila secundária para onde as mensagens são enviadas após um número configurado de falhas de processamento. Isso permite isolar mensagens problemáticas para análise posterior, sem bloquear a fila principal.

## 2. Amazon SNS (Simple Notification Service)

O SNS é um serviço de mensagens gerenciado que permite o envio de notificações do tipo "publicar/assinar" (pub/sub).

-   **Conceito Principal:** Um produtor (publisher) envia uma mensagem para um **Tópico (Topic)**. Múltiplos consumidores (subscribers) podem se inscrever nesse tópico e receberão uma cópia da mensagem.
-   **Modelo "Fan-Out":** Uma única mensagem publicada em um tópico SNS pode ser distribuída para múltiplos endpoints.
-   **Tipos de Assinantes (Subscribers):**
    -   Filas SQS
    -   Funções AWS Lambda
    -   Endpoints HTTP/HTTPS
    -   Endereços de e-mail
    -   Notificações push para dispositivos móveis (SMS, Apple, Google)
-   **Caso de Uso:** Notificar vários sistemas sobre um evento. Por exemplo, quando um pedido é criado, uma única mensagem no tópico "pedidos_novos" pode acionar uma função Lambda para processar o pedido, enviar um e-mail para o cliente e atualizar um painel de análise, tudo ao mesmo tempo.

## 3. Amazon API Gateway

O API Gateway é um serviço totalmente gerenciado que facilita a criação, publicação, manutenção, monitoramento e segurança de APIs em qualquer escala. Ele atua como a "porta de entrada" para suas aplicações.

-   **Tipos de API:**
    -   **API RESTful (REST API):** A mais comum, oferece um conjunto completo de recursos para gerenciamento de APIs.
    -   **API HTTP:** Uma alternativa mais leve, rápida e barata às APIs REST, ideal para cargas de trabalho serverless e backends HTTP.
    -   **API WebSocket:** Para aplicações de comunicação bidirecional em tempo real (ex: chats, painéis de streaming).
-   **Integração com o Backend:** O API Gateway pode se integrar com diversos tipos de backend:
    -   Funções AWS Lambda (integração mais comum)
    -   Qualquer endpoint HTTP público
    -   Outros serviços da AWS (ex: SQS, DynamoDB)
-   **Recursos Principais:**
    -   **Segurança:** Controle de acesso com IAM, Amazon Cognito e chaves de API.
    -   **Throttling (Limitação):** Protege seu backend contra picos de tráfego, definindo limites de taxa de requisições.
    -   **Cache:** Armazena em cache as respostas da API para reduzir a latência e a carga no backend.
    -   **Monitoramento:** Integração com CloudWatch para logs e métricas.

## 4. AWS Elastic Beanstalk

O Elastic Beanstalk é um serviço de orquestração que facilita a implantação e o escalonamento de aplicações e serviços web desenvolvidos em linguagens como Java, .NET, PHP, Node.js, Python, Ruby, Go e Docker.

-   **Conceito Principal:** Você simplesmente faz o upload do seu código, e o Elastic Beanstalk cuida automaticamente do provisionamento da infraestrutura, incluindo:
    -   Balanceamento de carga (Elastic Load Balancing)
    -   Escalonamento automático (Auto Scaling)
    -   Monitoramento da saúde da aplicação
    -   Provisionamento de instâncias EC2
-   **Nível de Abstração:** É um serviço de Plataforma como Serviço (PaaS). Ele oferece menos flexibilidade que provisionar os recursos manualmente (IaaS), mas é muito mais simples e rápido para colocar uma aplicação no ar. Você ainda tem acesso à configuração subjacente se precisar.
-   **Modelos de Implantação (Deployment Policies):**
    -   **All at once:** Implanta a nova versão em todas as instâncias de uma vez. Rápido, mas causa indisponibilidade.
    -   **Rolling:** Implanta a nova versão em lotes de instâncias, mantendo o restante em serviço. Reduz a capacidade durante a implantação.
    -   **Rolling with additional batch:** Lança um novo lote de instâncias para a implantação, mantendo a capacidade total.
    -   **Immutable:** Lança um novo conjunto completo de instâncias com a nova versão em um novo Auto Scaling Group e, após a verificação de saúde, troca o tráfego para as novas instâncias. Mais seguro e sem impacto na capacidade.
    -   **Blue/Green:** Implanta a nova versão em um ambiente separado e troca o tráfego via DNS (requer configuração manual).
-   **Caso de Uso:** Ideal para desenvolvedores que querem focar no código e não no gerenciamento da infraestrutura. Perfeito para aplicações web tradicionais.

## 5. Amazon Cognito

O Amazon Cognito permite adicionar inscrição, login e controle de acesso de usuários às suas aplicações web e móveis de forma rápida e fácil.

-   **Conceito Principal:** É um serviço de identidade totalmente gerenciado. Ele lida com toda a complexidade de autenticação, autorização e gerenciamento de usuários.
-   **Componentes Principais:**
    -   **User Pools (Grupos de Usuários):** São diretórios de usuários. Um User Pool permite que os usuários se inscrevam e façam login na sua aplicação. Ele pode ser um provedor de identidade autônomo ou pode federar com provedores de identidade de terceiros (como Google, Facebook, Apple, SAML 2.0).
    -   **Identity Pools (Grupos de Identidades):** Permitem conceder aos seus usuários (autenticados ou anônimos) acesso temporário a outros serviços da AWS. Após um usuário ser autenticado por um User Pool ou um provedor de identidade de terceiros, o Identity Pool troca o token de identidade por credenciais temporárias da AWS com permissões limitadas que você define via IAM Roles.
-   **Caso de Uso:** Proteger o acesso a uma API no API Gateway, permitir que usuários de uma aplicação móvel salvem dados no DynamoDB de forma segura, ou adicionar login social (Facebook, Google) à sua aplicação web.

## 6. AWS Step Functions

O Step Functions é um serviço de orquestração serverless que permite sequenciar funções AWS Lambda e múltiplos serviços da AWS em fluxos de trabalho visualmente intuitivos.

-   **Conceito Principal:** Você define seus fluxos de trabalho como **Máquinas de Estado (State Machines)**. Cada etapa (State) no seu fluxo de trabalho pode ser uma função Lambda, uma interação com SQS, SNS, DynamoDB, ou outros serviços.
-   **Recursos:**
    -   **Sequenciamento:** Executa tarefas em sequência.
    -   **Paralelismo:** Executa ramos de tarefas em paralelo.
    -   **Condicionais:** Escolhe qual etapa executar com base na saída da etapa anterior.
    -   **Tratamento de Erros:** Permite `try/catch/finally` para lidar com falhas e executar lógicas de repetição (retry).
-   **Caso de Uso:** Orquestrar um processo de pedido complexo: validar o pedido, processar o pagamento, iniciar o envio e enviar notificações. Se qualquer etapa falhar, o Step Functions pode reverter a transação ou notificar um administrador.

## 7. Amazon EventBridge

O EventBridge é um barramento de eventos (event bus) serverless que facilita a conexão de aplicações usando dados de suas próprias aplicações, de aplicações SaaS (Software-as-a-Service) e de serviços da AWS.

-   **Conceito Principal:** É uma evolução do CloudWatch Events, mas com mais funcionalidades. Ele permite que sistemas desacoplados se comuniquem através de eventos. Um serviço de origem emite um evento para o barramento de eventos, e o EventBridge usa **Regras (Rules)** para filtrar e enviar esses eventos para um ou mais **Alvos (Targets)**.
-   **Componentes:**
    -   **Event Bus:** O barramento que recebe os eventos. Existe um barramento padrão (default) que recebe eventos de serviços da AWS. Você pode criar barramentos personalizados para suas aplicações.
    -   **Rules:** Filtram os eventos recebidos com base em seu conteúdo (o `event pattern`).
    -   **Targets:** O que é invocado quando uma regra corresponde a um evento. Alvos podem ser funções Lambda, filas SQS, tópicos SNS, máquinas de estado do Step Functions, e muitos outros.
-   **Diferença para o SNS:** Enquanto o SNS é ótimo para "fan-out" simples (uma mensagem para muitos assinantes), o EventBridge é mais poderoso para roteamento complexo baseado no conteúdo do evento, permitindo que diferentes alvos reajam a diferentes tipos de eventos vindos da mesma fonte.
