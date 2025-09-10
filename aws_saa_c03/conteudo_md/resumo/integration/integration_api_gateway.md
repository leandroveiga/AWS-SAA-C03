### O que é Amazon API Gateway

O Amazon API Gateway é um serviço totalmente gerenciado que facilita para os desenvolvedores a criação, publicação, manutenção, monitoramento e segurança de APIs em qualquer escala. Ele atua como uma "porta da frente" para que as aplicações acessem dados, lógica de negócios ou funcionalidades de seus serviços de backend, como cargas de trabalho em execução no Amazon EC2, código em execução no AWS Lambda ou qualquer serviço da web.

### Como funciona o Amazon API Gateway

1.  **Criação de uma API:** Você define uma API no API Gateway. A API é uma coleção de recursos e métodos.
2.  **Definição de Recursos e Métodos:**
    *   **Recursos:** São as entidades da sua API, representadas por caminhos de URL (ex: `/users`, `/orders`).
    *   **Métodos:** Correspondem aos verbos HTTP (GET, POST, PUT, DELETE) que podem ser executados em um recurso.
3.  **Integração com o Backend (Integration):** Para cada método, você configura uma "integração" que define como o API Gateway deve passar a solicitação para o seu serviço de backend. Os tipos de integração incluem:
    *   **AWS Lambda:** Invoca uma função Lambda (o caso de uso mais comum para APIs sem servidor).
    *   **HTTP:** Encaminha a solicitação para um endpoint HTTP (ex: um Application Load Balancer ou um serviço de terceiros).
    *   **AWS Service:** Integra-se diretamente com outros serviços da AWS (ex: publicar em um tópico SNS ou escrever em uma tabela DynamoDB).
    *   **VPC Link:** Permite conectar-se a recursos privados dentro de uma VPC.
4.  **Implantação (Deployment):** Depois de configurar sua API, você a "implanta" em um "estágio" (stage), como `dev`, `test` ou `prod`. Cada estágio tem uma URL de invocação única.
5.  **Execução:** Quando um cliente faz uma chamada para a URL da sua API, o API Gateway recebe a solicitação, executa as validações e verificações de autorização configuradas, transforma a solicitação se necessário e a encaminha para o backend integrado. Em seguida, ele recebe a resposta do backend, a transforma e a retorna ao cliente.

### Tipos de API

*   **RESTful API (API REST):** Uma coleção de recursos e métodos HTTP. Oferece um conjunto completo de recursos, como transformação de dados, autorização e limitação de taxa.
*   **HTTP API:** Uma alternativa mais leve, de menor latência e mais barata às APIs REST. É ideal para criar APIs que fazem proxy para funções Lambda ou backends HTTP. Oferece um conjunto principal de recursos, mas não todos os recursos das APIs REST.
*   **WebSocket API:** Permite criar aplicações de comunicação bidirecional em tempo real, como painéis de controle ao vivo ou aplicações de chat. O servidor pode enviar mensagens para os clientes conectados.

### Recursos Principais

*   **Segurança e Autorização:**
    *   **Autenticação:** Suporta vários mecanismos, incluindo chaves de API, autenticação do IAM, autorizadores Lambda (lógica personalizada) e Amazon Cognito User Pools.
    *   **Limitação de Taxa (Throttling):** Protege seu backend de picos de tráfego, definindo limites de taxa de solicitação por cliente.
*   **Gerenciamento do Ciclo de Vida:** Suporta vários estágios e versões, permitindo que você teste, implante e reverta alterações de forma controlada.
*   **Cache:** Pode armazenar em cache as respostas do seu endpoint para reduzir o número de chamadas feitas ao seu backend e melhorar a latência das solicitações.
*   **Monitoramento:** Integra-se com o Amazon CloudWatch para monitorar métricas de desempenho (como número de chamadas, latência) e erros. Os logs de execução podem ser enviados para o CloudWatch Logs.

### Benefícios do API Gateway

*   **Totalmente Gerenciado e Escalável:** Lida com todo o trabalho pesado envolvido na aceitação e processamento de centenas de milhares de chamadas de API simultâneas.
*   **Desenvolvimento Eficiente de API:** Permite executar várias versões da mesma API simultaneamente, permitindo que você itere, teste e lance novas versões rapidamente.
*   **Desempenho em Escala:** Aproveita a rede global do CloudFront para fornecer baixa latência aos usuários finais.
*   **Custo-Benefício:** Com o modelo de preços pago conforme o uso, você paga apenas pelas chamadas de API que recebe e pela quantidade de dados transferidos.
