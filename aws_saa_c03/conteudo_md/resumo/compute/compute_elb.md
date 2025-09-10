### O que é AWS Elastic Load Balancing (ELB)

O AWS Elastic Load Balancing (ELB) é um serviço da Amazon Web Services que distribui automaticamente o tráfego de entrada de aplicações por múltiplos destinos — como instâncias Amazon EC2, contêineres e endereços IP — em uma ou mais Zonas de Disponibilidade. Ele aumenta a tolerância a falhas e a escalabilidade de suas aplicações, garantindo que nenhum destino único seja sobrecarregado.

### Como funciona o Elastic Load Balancing

1.  **Recebe o Tráfego:** O load balancer atua como um ponto único de contato para os clientes. Ele recebe todas as solicitações de entrada para sua aplicação através de um nome DNS.
2.  **Verifica a Saúde dos Destinos:** O ELB realiza verificações de saúde (health checks) contínuas nos destinos registrados (por exemplo, instâncias EC2) para garantir que eles estejam operando corretamente.
3.  **Distribui a Carga:** Com base nos resultados das verificações de saúde, o load balancer distribui o tráfego de entrada apenas para os destinos considerados saudáveis, seguindo um algoritmo de roteamento.
4.  **Escala Automaticamente:** O próprio load balancer é elástico e escala sua capacidade de manipulação de tráfego para cima ou para baixo em resposta ao tráfego de entrada, sem a necessidade de intervenção manual.

### Tipos de Load Balancer

*   **Application Load Balancer (ALB):** Ideal para tráfego HTTP e HTTPS (Camada 7). Permite roteamento avançado com base em regras, como o caminho da URL (`/imagens`, `/api`) ou o nome do host.
*   **Network Load Balancer (NLB):** Projetado para altíssimo desempenho em tráfego TCP, UDP e TLS (Camada 4). É capaz de lidar com milhões de solicitações por segundo, mantendo latências ultrabaixas.
*   **Gateway Load Balancer (GWLB):** Permite implantar, escalar e gerenciar appliances virtuais de terceiros, como firewalls e sistemas de prevenção de intrusão. Opera na Camada 3 (Rede) e 4 (Transporte).
*   **Classic Load Balancer (CLB):** É o load balancer da geração anterior. A AWS recomenda o uso de ALBs ou NLBs para novas aplicações.

### Benefícios do Elastic Load Balancing

*   **Alta Disponibilidade:** Aumenta a tolerância a falhas da aplicação ao distribuir o tráfego e redirecioná-lo automaticamente para longe de destinos insalubres.
*   **Escalabilidade:** Lida de forma transparente com picos de tráfego e funciona perfeitamente com o EC2 Auto Scaling.
*   **Segurança:** Oferece recursos de segurança integrados, como o descarregamento de criptografia SSL/TLS e integração com o AWS WAF.
*   **Flexibilidade:** Suporta diferentes tipos de tráfego e oferece roteamento avançado para arquiteturas de microsserviços.
