### O que é Amazon CloudFront

O Amazon CloudFront é um serviço de rede de entrega de conteúdo (CDN) rápido que entrega dados, vídeos, aplicações e APIs de forma segura para clientes em todo o mundo com baixa latência e altas velocidades de transferência. O CloudFront é integrado com a AWS – tanto os locais físicos quanto outros serviços da AWS. Ele funciona perfeitamente com serviços como AWS Shield para mitigação de DDoS, Amazon S3, Elastic Load Balancing ou Amazon EC2 como origens para suas aplicações.

### Como funciona o Amazon CloudFront

1.  **Origem (Origin):** Você especifica a origem de onde o CloudFront buscará seu conteúdo. A origem pode ser um bucket do Amazon S3, um Elastic Load Balancer, uma instância EC2 ou qualquer servidor web HTTP personalizado.
2.  **Distribuição (Distribution):** Você cria uma "distribuição" do CloudFront, que informa ao serviço de onde entregar o conteúdo e como rastrear e gerenciar as solicitações.
3.  **Pontos de Presença (Edge Locations):** O CloudFront possui uma rede global de pontos de presença (também conhecidos como "edge locations"). Estes são data centers localizados em cidades ao redor do mundo.
4.  **Cache de Conteúdo:** Quando um usuário solicita seu conteúdo pela primeira vez, o CloudFront busca o conteúdo na sua origem e o armazena em cache no ponto de presença mais próximo do usuário.
5.  **Entrega a partir do Cache:** Nas solicitações subsequentes para o mesmo conteúdo feitas por usuários na mesma região geográfica, o CloudFront entrega o conteúdo diretamente do cache no ponto de presença, em vez de voltar à origem. Isso resulta em uma latência muito menor e melhor desempenho para o usuário final.
6.  **TTL (Time to Live):** Você controla por quanto tempo seu conteúdo permanece em cache nos pontos de presença configurando o TTL. Após o TTL expirar, na próxima vez que um usuário solicitar o conteúdo, o CloudFront retornará à origem para verificar se há uma versão mais recente.

### Recursos Principais

*   **Segurança:**
    *   **Integração com AWS Shield:** Todas as distribuições do CloudFront são protegidas gratuitamente pelo AWS Shield Standard contra ataques DDoS de camada de rede e transporte.
    *   **Integração com AWS WAF:** Você pode usar o Web Application Firewall (WAF) para proteger suas aplicações contra exploits da web comuns.
    *   **HTTPS/SSL:** Permite que você entregue seu conteúdo de forma segura usando HTTPS. Você pode usar certificados SSL/TLS gratuitos do AWS Certificate Manager (ACM).
    *   **Origens Seguras:** O CloudFront pode ser configurado para se comunicar com suas origens usando HTTPS.
    *   **Restrição de Acesso:** Você pode restringir o acesso ao seu conteúdo usando recursos como Signed URLs, Signed Cookies e Origin Access Identity (OAI) para conteúdo no S3.
*   **Conteúdo Dinâmico e Estático:** O CloudFront acelera a entrega de conteúdo estático (imagens, CSS, JavaScript) e dinâmico (APIs, renderização do lado do servidor).
*   **Lambda@Edge:** Permite executar funções do AWS Lambda em resposta a eventos do CloudFront. Isso permite que você execute código mais perto de seus usuários, o que melhora o desempenho e reduz a latência. Casos de uso incluem manipulação de cabeçalhos, otimização de imagens e autenticação/autorização de solicitações.

### Benefícios do CloudFront

*   **Desempenho e Baixa Latência:** Acelera a entrega de conteúdo para usuários em todo o mundo, melhorando a experiência do usuário.
*   **Redução da Carga na Origem:** Ao servir conteúdo a partir do cache, ele reduz o número de solicitações que sua origem precisa atender, diminuindo a carga e os custos.
*   **Segurança Integrada:** Oferece múltiplos níveis de proteção para suas aplicações e conteúdo.
*   **Custo-Benefício:** Pague apenas pelos dados que você transfere, sem taxas mínimas. A transferência de dados da origem da AWS para o CloudFront é gratuita.
*   **Integração Profunda com a AWS:** Funciona perfeitamente com o ecossistema da AWS.
