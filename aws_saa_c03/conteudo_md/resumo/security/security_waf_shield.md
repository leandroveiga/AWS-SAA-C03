### O que é AWS WAF e AWS Shield

O AWS WAF e o AWS Shield são dois serviços de segurança que trabalham juntos para proteger suas aplicações web e APIs contra vários tipos de ataques cibernéticos.

---

### AWS WAF (Web Application Firewall)

O AWS WAF é um firewall de aplicação web que ajuda a proteger suas aplicações contra exploits da web comuns que podem afetar a disponibilidade, comprometer a segurança ou consumir recursos excessivos. Ele funciona na camada de aplicação (Camada 7) do modelo OSI.

#### Como funciona o AWS WAF

1.  **Inspeção de Tráfego HTTP/S:** O WAF inspeciona as solicitações da web que chegam aos seus recursos protegidos, como um Application Load Balancer, Amazon CloudFront ou API Gateway.
2.  **Regras e Condições:** Você cria regras que verificam partes específicas das solicitações da web, como:
    *   Endereços IP de origem.
    *   Cabeçalhos HTTP.
    *   Corpo da solicitação.
    *   Strings de consulta da URI.
3.  **Ações:** Com base nas regras, o WAF pode:
    *   **ALLOW (Permitir):** Deixar a solicitação passar para o seu recurso.
    *   **BLOCK (Bloquear):** Bloquear a solicitação, retornando um código de status HTTP 403 (Forbidden).
    *   **COUNT (Contar):** Contar as solicitações que correspondem à regra, mas permitir que passem. Útil para testar regras antes de aplicá-las.
4.  **Web ACLs (Access Control Lists):** As regras são combinadas em uma "Web ACL". Você associa a Web ACL ao seu recurso da AWS para protegê-lo.
5.  **Regras Gerenciadas (Managed Rules):** A AWS e parceiros do AWS Marketplace oferecem conjuntos de regras pré-configuradas para proteger contra ameaças comuns, como as listadas no OWASP Top 10 (ex: injeção de SQL, Cross-Site Scripting - XSS).

---

### AWS Shield

O AWS Shield é um serviço gerenciado de proteção contra ataques de Negação de Serviço Distribuída (DDoS). Ele protege as aplicações em execução na AWS. O Shield opera nas camadas de rede (Camada 3) e transporte (Camada 4).

Existem dois níveis de AWS Shield:

#### 1. AWS Shield Standard
*   **O que é:** Uma proteção automática e gratuita que é ativada por padrão para todos os clientes da AWS.
*   **Como funciona:** Defende contra os ataques DDoS mais comuns e frequentes que visam seu site ou aplicações. Ele fornece proteção sempre ativa, com detecção e mitigação em linha para garantir que a latência não aumente.
*   **Recursos Protegidos:** Protege serviços como Amazon CloudFront, Amazon Route 53 e Elastic Load Balancing.

#### 2. AWS Shield Advanced
*   **O que é:** Um serviço pago que oferece proteções adicionais e mais sofisticadas para aplicações em execução no Amazon EC2, Elastic Load Balancing, CloudFront, Route 53 e Global Accelerator.
*   **Recursos Adicionais:**
    *   **Detecção e Mitigação Aprimoradas:** Proteção contra ataques DDoS maiores e mais sofisticados.
    *   **Visibilidade de Ataques:** Relatórios quase em tempo real e detalhados sobre os ataques.
    *   **Integração com WAF:** O Shield Advanced pode criar regras do WAF automaticamente em resposta a ataques na camada de aplicação.
    *   **Acesso 24/7 ao Time de Resposta do Shield (SRT):** Acesso a especialistas da AWS durante um ataque para ajudar na mitigação.
    *   **Proteção de Custos (Cost Protection):** Fornece créditos contra cobranças resultantes de picos de uso do EC2, ELB, CloudFront e Route 53 que possam ser causados por um ataque DDoS.

### Como WAF e Shield trabalham juntos

*   O **AWS Shield** protege contra ataques volumétricos de infraestrutura (Camadas 3 e 4), como inundações SYN ou UDP.
*   O **AWS WAF** protege contra ataques na camada de aplicação (Camada 7), como injeção de SQL ou XSS.

Usar ambos os serviços fornece uma defesa em profundidade abrangente para suas aplicações web. O Shield lida com os ataques de inundação em massa, enquanto o WAF filtra as solicitações maliciosas que visam explorar vulnerabilidades no seu código.
