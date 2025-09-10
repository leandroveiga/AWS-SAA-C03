### O que é Amazon Route 53

O Amazon Route 53 é um serviço da web de Sistema de Nomes de Domínio (DNS) em nuvem, altamente disponível e escalável. Ele foi projetado para oferecer aos desenvolvedores e empresas uma maneira extremamente confiável e econômica de rotear os usuários finais para aplicações da Internet, traduzindo nomes de domínio legíveis por humanos (como `www.example.com`) em endereços IP numéricos (como `192.0.2.1`) que os computadores usam para se conectar uns aos outros.

### Como funciona o Amazon Route 53

1.  **Registro de Domínio:** Você pode comprar e gerenciar nomes de domínio (como `example.com`) diretamente através do Route 53.
2.  **Criação de uma Zona Hospedada (Hosted Zone):** Quando você registra um domínio ou deseja gerenciar o DNS de um domínio existente, você cria uma "Hosted Zone". Uma Hosted Zone é um contêiner para os registros DNS do seu domínio.
3.  **Criação de Conjuntos de Registros (Record Sets):** Dentro da Hosted Zone, você cria registros que definem como o tráfego do seu domínio e subdomínios deve ser roteado.
    *   **Tipos de Registro Comuns:** `A` (mapeia um nome para um endereço IPv4), `AAAA` (para IPv6), `CNAME` (mapeia um nome para outro nome de domínio), `MX` (para servidores de e-mail), `TXT` (para verificação de domínio, etc.).
4.  **Resolução de DNS:** Quando um usuário digita seu nome de domínio em um navegador, a solicitação de DNS é enviada para a rede global de servidores DNS do Route 53. O Route 53 responde com o endereço IP apropriado com base nos registros que você configurou, direcionando o navegador do usuário para o seu site ou aplicação.

### Políticas de Roteamento (Routing Policies)

O Route 53 vai além do DNS padrão, oferecendo políticas de roteamento avançadas:

*   **Roteamento Simples (Simple Routing):** A configuração padrão. Roteia o tráfego para um único recurso, por exemplo, um servidor web.
*   **Roteamento Ponderado (Weighted Routing):** Permite associar vários recursos a um único nome de domínio e distribuir o tráfego entre eles com base em pesos que você define. Útil para testes A/B ou para enviar uma pequena porção do tráfego para um novo endpoint.
*   **Roteamento por Latência (Latency-based Routing):** Roteia os usuários para a região da AWS que oferece a menor latência para eles. Melhora o desempenho global para uma base de usuários distribuída geograficamente.
*   **Roteamento por Geolocalização (Geolocation Routing):** Permite escolher para onde o tráfego será roteado com base na localização geográfica de seus usuários (continente, país ou estado nos EUA). Útil para localizar conteúdo e restringir a distribuição.
*   **Roteamento por Geoproximidade (Geoproximity Routing):** Roteia o tráfego com base na localização geográfica de seus recursos e, opcionalmente, permite deslocar o tráfego de recursos em uma localização para outra (usando "biases").
*   **Roteamento de Failover (Failover Routing):** Permite configurar um failover ativo-passivo. O Route 53 monitora a saúde de seu endpoint primário e roteia o tráfego para um endpoint de recuperação de desastres se o primário se tornar insalubre.
*   **Roteamento de Múltiplos Valores (Multivalue Answer Routing):** Permite que o Route 53 responda a consultas de DNS com até oito registros saudáveis selecionados aleatoriamente. É uma forma simples de fazer balanceamento de carga do lado do cliente.

### Benefícios do Route 53

*   **Alta Disponibilidade e Confiabilidade:** Construído sobre a infraestrutura global e altamente confiável da AWS, com um SLA de 100% de disponibilidade.
*   **Flexibilidade e Inteligência:** As políticas de roteamento avançadas oferecem controle granular sobre como o tráfego é direcionado.
*   **Integração com a AWS:** Integra-se perfeitamente com outros serviços da AWS, como Elastic Load Balancers, instâncias EC2 e buckets S3.
*   **Rápido:** Usa uma rede global de servidores DNS para responder às consultas dos usuários a partir da localização mais próxima, reduzindo a latência.
