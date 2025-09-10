# Alta Disponibilidade e Escalabilidade: ELB e Auto Scaling

Para construir uma arquitetura robusta na AWS, é essencial garantir que sua aplicação seja tanto **altamente disponível** (resiliente a falhas) quanto **escalável** (capaz de lidar com variações na demanda). Os dois serviços principais para alcançar isso são o Elastic Load Balancing (ELB) e o EC2 Auto Scaling.

---

## Elastic Load Balancing (ELB)

O **ELB** distribui automaticamente o tráfego de entrada de aplicações por múltiplos destinos, como instâncias Amazon EC2, contêineres, endereços IP e funções Lambda. Ele aumenta a disponibilidade e a tolerância a falhas de suas aplicações.

<p align="center">
    <img src="https://docs.aws.amazon.com/pt_br/elasticloadbalancing/latest/application/images/component_architecture.png" alt="Componentes de um Application Load Balancer: clientes, listeners, regras e grupos de destino" width="650" />
    <br/>
    <em>Fonte: Documentação oficial AWS Elastic Load Balancing</em>
</p>

### Tipos de Load Balancers

A escolha do tipo de load balancer é um ponto crucial no design da arquitetura.

#### 1. Application Load Balancer (ALB)
-   **Camada:** 7 (Aplicação - HTTP/HTTPS).
-   **Inteligência:** É "inteligente". Ele pode inspecionar o conteúdo da requisição (como cabeçalhos, caminhos de URL, strings de consulta) e tomar decisões de roteamento com base nele.
-   **Roteamento:** Suporta roteamento baseado em caminho (`exemplo.com/imagens` vs. `exemplo.com/api`), roteamento baseado em host (`imagens.exemplo.com` vs. `api.exemplo.com`) e outras regras avançadas.
-   **Caso de Uso:** Ideal para arquiteturas de microsserviços, aplicações web modernas e qualquer cenário que precise de roteamento flexível.

#### 2. Network Load Balancer (NLB)
-   **Camada:** 4 (Transporte - TCP/UDP/TLS).
-   **Performance:** É "burro", mas **extremamente rápido**. Ele opera na camada de conexão e simplesmente encaminha os pacotes para os destinos. Oferece performance ultra-alta e latência muito baixa.
-   **Endereço IP:** Fornece um endereço IP estático por Zona de Disponibilidade.
-   **Caso de Uso:** Aplicações que exigem performance extrema, como jogos online, streaming de vídeo, ou qualquer aplicação TCP/UDP de alto tráfego. Também é ideal quando um IP estático é necessário.

#### 3. Gateway Load Balancer (GWLB)
-   **Camada:** 3 (Rede - IP).
-   **Função:** Permite implantar, escalar e gerenciar appliances virtuais de terceiros, como firewalls, sistemas de detecção e prevenção de intrusão (IDS/IPS) e sistemas de inspeção profunda de pacotes.
-   **Caso de Uso:** Inserir dispositivos de segurança de rede de forma transparente no caminho do tráfego.

#### Classic Load Balancer (CLB) - *Legado*
-   **Camada:** 4 (TCP/SSL) e 7 (HTTP/HTTPS).
-   **Status:** É da geração anterior. Embora ainda seja suportado, **não é recomendado para novas aplicações**. Os ALBs e NLBs oferecem muito mais funcionalidades.

---

## EC2 Auto Scaling

O **EC2 Auto Scaling** ajuda a garantir que você tenha o número correto de instâncias Amazon EC2 disponíveis para lidar com a carga de sua aplicação.

<p align="center">
    <img src="https://docs.aws.amazon.com/pt_br/autoscaling/ec2/userguide/images/asg-basic-arch.png" alt="Arquitetura básica de um Auto Scaling Group distribuindo instâncias em múltiplas zonas de disponibilidade" width="600" />
    <br/>
    <em>Fonte: Documentação oficial AWS EC2 Auto Scaling</em>
</p>

### Componentes do Auto Scaling

-   **Launch Template / Launch Configuration:** Define o que será lançado. Especifica a AMI, o tipo de instância, o par de chaves, os security groups, etc. **Launch Templates são a forma mais nova e recomendada.**
-   **Auto Scaling Group (ASG):** O núcleo do serviço. Define onde lançar as instâncias (VPC e sub-redes), a qual load balancer se registrar, e os limites de escalabilidade.
    -   **Min Size:** O número mínimo de instâncias que o ASG manterá em execução.
    -   **Max Size:** O número máximo de instâncias para o qual o ASG pode escalar.
    -   **Desired Capacity:** O número de instâncias que o ASG tentará manter. Se não houver política de escalabilidade, ele manterá esse número.
-   **Scaling Policies (Políticas de Escalabilidade):** Define quando escalar.
    -   **Target Tracking Scaling:** A mais simples e recomendada. Você define uma métrica e um valor alvo (ex: "manter a utilização média da CPU em 50%"). O Auto Scaling cuida do resto.
    -   **Simple/Step Scaling:** Políticas mais antigas que escalam em resposta a um alarme do CloudWatch (ex: "se a CPU > 70%, adicione 2 instâncias").
    -   **Scheduled Scaling:** Escala com base em uma programação (ex: "aumente a capacidade para 10 instâncias toda sexta-feira às 18h").

### Scaling Up vs. Scaling Out

-   **Scaling Up (Vertical):** Aumentar o tamanho de uma instância (ex: de `t2.micro` para `t2.large`). Geralmente requer uma parada e reinicialização, causando indisponibilidade.
-   **Scaling Out (Horizontal):** Adicionar mais instâncias. É a abordagem usada pelo Auto Scaling e é fundamental para a alta disponibilidade e elasticidade na nuvem.

O ELB e o Auto Scaling trabalham juntos para criar uma arquitetura auto-reparável e elástica. O ELB distribui o tráfego entre as instâncias, e o Auto Scaling garante que o número de instâncias se ajuste dinamicamente à demanda.
