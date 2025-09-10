# DNS, Cache e Performance na AWS

Para entregar aplicações com baixa latência e alta performance para usuários em todo o mundo, a AWS oferece um conjunto de serviços de borda, incluindo o Amazon Route 53 para DNS e o Amazon CloudFront como uma Content Delivery Network (CDN).

---

## Amazon Route 53

O **Route 53** é um serviço de Sistema de Nomes de Domínio (DNS) da web, altamente disponível e escalável. Ele foi projetado para oferecer aos desenvolvedores e empresas uma maneira extremamente confiável e econômica de rotear os usuários finais para aplicações da Internet.

### Funcionalidades Principais

-   **Registro de Domínio:** Você pode comprar e gerenciar nomes de domínio diretamente no Route 53.
-   **Serviço de DNS:** Traduz nomes de domínio amigáveis (como `www.exemplo.com`) para os endereços IP numéricos (como `192.0.2.1`) que os computadores usam para se conectar uns aos outros.
-   **Verificações de Saúde (Health Checks):** O Route 53 pode monitorar a saúde e a performance de sua aplicação, servidores web e outros recursos.
-   **Roteamento de Tráfego:** O Route 53 oferece várias políticas de roteamento para controlar como ele responde às consultas de DNS.

### Políticas de Roteamento do Route 53

Este é um tópico **fundamental** para o exame.

| Política de Roteamento | Como Funciona | Caso de Uso Principal |
| :--- | :--- | :--- |
| **Simple** | Responde com um ou mais valores em ordem aleatória. Não suporta health checks. | Um único servidor web ou recurso. |
| **Weighted (Ponderado)** | Distribui o tráfego entre múltiplos recursos com base em pesos que você define (ex: 80% para A, 20% para B). | Testes A/B, Blue/Green deployments. |
| **Latency (Latência)** | Roteia o tráfego para o recurso na região da AWS que fornece a menor latência para o usuário solicitante. | Aplicações globais onde a latência é o fator mais crítico. |
| **Failover** | Roteia o tráfego para um recurso primário quando ele está saudável e para um recurso secundário (de backup) se o primário falhar. | Arquiteturas de recuperação de desastres (DR) ativas-passivas. |
| **Geolocation (Geolocalização)**| Roteia o tráfego com base na localização geográfica do usuário (continente, país ou estado nos EUA). | Restringir a distribuição de conteúdo, apresentar o site no idioma correto. |
| **Geoproximity (Proximidade Geográfica)** | Roteia o tráfego com base na localização geográfica de seus recursos e, opcionalmente, desloca o tráfego de recursos em uma localização para outra (usando "bias"). | Balanceamento de carga de tráfego entre regiões, movendo o tráfego para longe de um recurso sobrecarregado. |
| **Multivalue Answer (Múltiplos Valores)** | Responde a consultas de DNS com até oito registros saudáveis selecionados aleatoriamente. É como o roteamento simples, mas com health checks. | Melhorar a disponibilidade e o balanceamento de carga no lado do cliente. |

---

## Amazon CloudFront

O **CloudFront** é a **Content Delivery Network (CDN)** global da AWS. Uma CDN acelera a entrega de seu conteúdo estático e dinâmico (como `.html`, `.css`, `.js`, imagens e vídeos) para os usuários.

### Como o CloudFront Funciona

1.  Um usuário solicita seu conteúdo.
2.  A solicitação é roteada para o **Ponto de Presença (Edge Location)** do CloudFront mais próximo do usuário, em termos de latência.
3.  O CloudFront verifica se o conteúdo está em seu **cache** no Ponto de Presença.
    -   **Cache Hit (Acerto de Cache):** Se o conteúdo estiver no cache, o CloudFront o entrega diretamente ao usuário, resultando em baixa latência.
    -   **Cache Miss (Falta de Cache):** Se o conteúdo não estiver no cache, o CloudFront encaminha a solicitação para sua **origem** (ex: um bucket S3, um Application Load Balancer ou um servidor web EC2).
4.  A origem envia o conteúdo de volta para o Ponto de Presença.
5.  O CloudFront armazena o conteúdo em cache (para solicitações futuras) e o entrega ao usuário.

### Principais Benefícios

-   **Performance:** Reduz a latência ao servir conteúdo de um local próximo ao usuário.
-   **Segurança:** Integra-se com o AWS Shield (para proteção contra DDoS) e o AWS WAF (Web Application Firewall) para proteger sua aplicação na borda.
-   **Redução de Carga na Origem:** Como o conteúdo é servido do cache, isso reduz o número de solicitações que chegam aos seus servidores de origem, diminuindo a carga e os custos.

### OAI (Origin Access Identity)

-   Uma **OAI** é uma identidade especial do CloudFront que você pode usar para restringir o acesso ao conteúdo em um bucket S3.
-   Ao usar uma OAI, você pode configurar a política do seu bucket S3 para permitir o acesso **apenas** ao CloudFront. Isso impede que os usuários acessem seus arquivos diretamente pela URL do S3, forçando-os a usar as URLs do CloudFront. É uma prática de segurança essencial ao usar o S3 como origem.

---

## AWS Global Accelerator

O **Global Accelerator** é um serviço de rede que melhora a disponibilidade e a performance de suas aplicações com usuários globais.

-   **Como Funciona:** Ele fornece dois endereços IP estáticos que atuam como um ponto de entrada fixo para suas aplicações. O tráfego dos usuários entra na rede global da AWS no Ponto de Presença mais próximo e viaja pela rede congestionamento-livre da AWS até seus endpoints (ALBs, NLBs, EC2s).
-   **Diferença para o CloudFront:**
    -   **CloudFront:** Ideal para conteúdo em cache (HTTP/HTTPS).
    -   **Global Accelerator:** Ideal para aplicações não-HTTP (como jogos, IoT) ou aplicações HTTP que precisam de IPs estáticos ou failover rápido e determinístico entre regiões, sem depender do cache de DNS.
