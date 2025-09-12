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

---

## 5. Amazon Cognito

O Amazon Cognito é um serviço que fornece autenticação, autorização e gerenciamento de usuários para suas aplicações web e móveis. Ele permite que você adicione o registro e o login de usuários de forma rápida e fácil.

-   **Principais Componentes:**
    -   **User Pools (Grupos de Usuários):** São diretórios de usuários. Um User Pool gerencia o registro, login, e o perfil dos usuários. Ele pode ser um provedor de identidade autônomo, com funcionalidades como recuperação de senha e autenticação multifator (MFA).
    -   **Identity Pools (Grupos de Identidades):** Permitem conceder aos seus usuários acesso a outros serviços da AWS. Após um usuário se autenticar (seja por um User Pool do Cognito ou por um provedor de identidade social como Google, Facebook, Apple), o Identity Pool fornece credenciais temporárias da AWS para que eles possam acessar recursos permitidos (ex: fazer upload de um arquivo para um bucket S3 específico).

-   **Como funciona:**
    1.  Um usuário se registra ou faz login através do seu User Pool.
    2.  Após a autenticação bem-sucedida, o Cognito retorna um JSON Web Token (JWT).
    3.  Sua aplicação pode usar esse token para se comunicar com o Identity Pool.
    4.  O Identity Pool troca o token por credenciais temporárias do AWS IAM.
    5.  A aplicação usa essas credenciais para interagir com os serviços da AWS em nome do usuário.

-   **Caso de Uso:**
    -   Adicionar funcionalidade de "Login com Google/Facebook" a uma aplicação móvel.
    -   Criar um portal web onde os usuários têm seu próprio login e senha para acessar conteúdo personalizado.
    -   Permitir que usuários de uma aplicação acessem diretamente e de forma segura recursos específicos da AWS, sem expor credenciais de longa duração.

---

## 6. AWS Step Functions

O Step Functions é um serviço de orquestração serverless que permite sequenciar funções AWS Lambda e múltiplos serviços da AWS em fluxos de trabalho visualmente intuitivos.

-   **Conceito Principal:** Você define seus fluxos de trabalho como **Máquinas de Estado (State Machines)**. Cada etapa (State) no seu fluxo de trabalho pode ser uma função Lambda, uma interação com SQS, SNS, DynamoDB, ou outros serviços.
-   **Recursos:**
    -   **Sequenciamento:** Executa tarefas em sequência.
    -   **Paralelismo:** Executa ramos de tarefas em paralelo.
    -   **Condicionais:** Escolhe qual etapa executar com base na saída da etapa anterior.
    -   **Tratamento de Erros:** Permite `try/catch/finally` para lidar com falhas e executar lógicas de repetição (retry).
-   **Caso de Uso:** Orquestrar um processo de pedido complexo: validar o pedido, processar o pagamento, iniciar o envio e enviar notificações. Se qualquer etapa falhar, o Step Functions pode reverter a transação ou notificar um administrador.

---

## 7. Amazon EventBridge

O Amazon EventBridge é um barramento de eventos (event bus) serverless que facilita a conexão de aplicações usando dados de suas próprias aplicações, aplicações SaaS (Software as a Service) e serviços da AWS. Ele é uma evolução do CloudWatch Events, com mais funcionalidades.

-   **Conceito Principal:** O EventBridge recebe eventos de uma **fonte**, aplica uma **regra** para filtrar os eventos e os roteia para um ou mais **Alvos (Targets)**.
-   **Componentes:**
    -   **Event Bus:** O barramento que recebe os eventos. Existe um barramento padrão (default) que recebe eventos de serviços da AWS. Você pode criar barramentos personalizados para suas aplicações.
    -   **Rules:** Filtram os eventos recebidos com base em seu conteúdo (o `event pattern`).
    -   **Targets:** O que é invocado quando uma regra corresponde a um evento. Alvos podem ser funções Lambda, filas SQS, tópicos SNS, máquinas de estado do Step Functions, e muitos outros.
    -   **Fontes:** Serviços da AWS (ex: EC2, S3), suas próprias aplicações (custom events) ou parceiros SaaS (ex: Zendesk, Shopify).
    -   **Regras:** Filtram os eventos com base em seu conteúdo. Por exemplo, uma regra pode corresponder a todos os eventos de `EC2 Instance State-change Notification` onde o estado é `terminated`.
    -   **Alvos:** O que é invocado quando uma regra corresponde a um evento. Os alvos podem ser funções Lambda, filas SQS, tópicos SNS, máquinas de estado do Step Functions e muitos outros serviços.

-   **EventBridge vs. SNS:**
    -   **SNS (Simple Notification Service):** É um serviço de pub/sub simples. Um produtor publica uma mensagem em um tópico e todos os assinantes recebem a mesma mensagem. A filtragem no lado do assinante é limitada.
    -   **EventBridge:** É um sistema de roteamento de eventos mais avançado. Ele permite uma filtragem complexa baseada no conteúdo do evento, permitindo que diferentes alvos reajam a diferentes tipos de eventos, mesmo que venham da mesma fonte. Ele também se integra nativamente com parceiros SaaS.

-   **Caso de Uso:**
    -   Quando um novo usuário se inscreve em uma aplicação SaaS (fonte), uma regra no EventBridge detecta o evento e aciona uma função Lambda (alvo) para criar um registro de boas-vindas em seu banco de dados.
    -   Quando uma instância EC2 é terminada, uma regra envia uma notificação para um tópico SNS para alertar os administradores.