# Conectividade na AWS

A conectividade de rede na AWS é fundamental para estabelecer comunicações seguras e eficientes entre a sua infraestrutura on-premises e a nuvem AWS, bem como para conectar recursos dentro da própria nuvem. A seguir, detalhamos os principais serviços de conectividade, organizados de forma lógica desde o acesso básico à internet até a conectividade híbrida complexa.

---

## Acesso à Internet a partir da VPC

### Internet Gateway (IGW)

O **Internet Gateway (IGW)** é um componente de VPC horizontalmente escalável, redundante e altamente disponível que permite a comunicação **bidirecional** entre instâncias na sua VPC e a internet. Ele serve a dois propósitos principais:

1.  Fornecer um alvo na tabela de rotas da sua VPC para o tráfego roteável pela internet.
2.  Realizar a tradução de endereços de rede (NAT) para instâncias que possuem endereços IPv4 públicos.

-   **Características:**
    -   Não impõe limites de largura de banda.
    -   É um recurso gerenciado pela AWS, garantindo alta disponibilidade.
    -   Para que uma instância em uma **sub-rede pública** acesse a internet, a tabela de rotas da sub-rede deve ter uma rota para o IGW (`0.0.0.0/0 -> igw-id`).

-   **Casos de Uso:**
    -   Permitir que servidores web em uma sub-rede pública recebam tráfego da internet.
    -   Permitir que instâncias com IP público acessem a internet para atualizações de software ou para se comunicar com serviços externos.

### NAT Gateway

O **NAT (Network Address Translation) Gateway** é um serviço gerenciado que permite que instâncias em uma **sub-rede privada** iniciem conexões de **saída** para a internet ou outros serviços da AWS, mas impede que a internet inicie uma conexão com essas instâncias.

-   **Características:**
    -   Altamente disponível e escalável (até 45 Gbps).
    -   Deve ser implantado em uma **sub-rede pública** (pois ele precisa de acesso à internet via IGW).
    -   Associado a um Elastic IP para ter um endereço de IP público fixo para o tráfego de saída.
    -   As tabelas de rotas das **sub-redes privadas** são configuradas para direcionar o tráfego destinado à internet (`0.0.0.0/0`) para o NAT Gateway.

-   **Casos de Uso:**
    -   Permitir que instâncias em sub-redes privadas (como bancos de dados) façam o download de patches e atualizações.
    -   Permitir que aplicações em sub-redes privadas acessem APIs públicas sem se exporem diretamente à internet.

### Diferenças-chave: Internet Gateway vs. NAT Gateway

| Característica | Internet Gateway (IGW) | NAT Gateway |
| :--- | :--- | :--- |
| **Direção do Tráfego** | **Bidirecional** (Entrada e Saída) | **Apenas Saída** (da VPC para a Internet) |
| **Propósito** | Permitir que recursos **públicos** acessem a Internet. | Permitir que recursos **privados** acessem a Internet. |
| **Localização** | Anexado à VPC. | Implantado em uma **sub-rede pública**. |
| **Segurança** | Permite conexões iniciadas pela Internet. | **Bloqueia** conexões iniciadas pela Internet. |
| **Requisito de IP** | Requer que a instância tenha um IP Público ou Elástico. | Não requer IP público na instância; ele mesmo usa um IP Elástico. |
| **Custo** | Sem custo pelo gateway, apenas pela transferência de dados. | Custo por hora de provisionamento + custo por GB processado. |

#### Exemplo de Arquitetura

```
Internet
    |
    v
+---------------------+
|  Internet Gateway   |
+---------------------+
    |
    v
--- VPC ------------------------------------------------
|                                                      |
|  --- Sub-rede Pública ----------------------------  |
| |                                                 | |
| |  +------------------+      +-----------------+  | |
| |  | Instância EC2    |      |  NAT Gateway    |  | |
| |  | (Servidor Web)   |----->|  (com EIP)      |  | |
| |  | (com IP Público) |      +-----------------+  | |
| |  +------------------+             ^             | |
| |                                   |             | |
|  -------------------------------------------------  |
|                                                      |
|  --- Sub-rede Privada -----------------------------  |
| |                                                 | |
| |  +------------------+                           | |
| |  | Instância EC2    |                           | |
| |  | (Banco de Dados) |---------------------------+ |
| |  +------------------+                           | |
| |                                                 | |
|  -------------------------------------------------  |
|                                                      |
-------------------------------------------------------
```
Neste cenário:
- O **Servidor Web** na sub-rede pública pode receber tráfego da Internet e responder diretamente através do **Internet Gateway**.
- O **Banco de Dados** na sub-rede privada não pode ser acessado pela Internet, mas pode iniciar conexões de saída (ex: para baixar atualizações) que são roteadas através do **NAT Gateway**.

---

## Conectividade entre VPCs

### VPC Peering

O **VPC Peering** permite conectar duas VPCs de forma privada usando o backbone da AWS. As instâncias em ambas as VPCs podem se comunicar como se estivessem na mesma rede.

-   **Características:**
    -   Não há um ponto único de falha para comunicação ou um gargalo de largura de banda.
    -   O tráfego sempre permanece na rede global da AWS e nunca atravessa a internet pública.
    -   Funciona entre VPCs na mesma conta ou em contas diferentes, e na mesma região ou em regiões diferentes (Inter-Region VPC Peering).
    -   **Não suporta roteamento transitivo:** se a VPC A tem peering com a VPC B e a VPC B tem peering com a VPC C, a VPC A não pode acessar a VPC C através da VPC B. Esta é a principal limitação que o Transit Gateway resolve.

-   **Casos de Uso:**
    -   Compartilhar recursos entre diferentes unidades de negócios com poucas VPCs.
    -   Centralizar serviços (como autenticação) em uma VPC e acessá-los a partir de outra.

---

## Conectividade Centralizada

### AWS Transit Gateway

O **AWS Transit Gateway (TGW)** atua como um **hub central** de trânsito de rede, simplificando a conectividade entre múltiplas VPCs, contas da AWS e redes on-premises. Ele elimina a necessidade de criar uma malha complexa de conexões de emparelhamento (peering).

-   **Benefícios:**
    -   **Gerenciamento Simplificado (Hub and Spoke):** Conecte milhares de VPCs e redes on-premises a um único gateway.
    -   **Roteamento Centralizado:** Controle como o tráfego é roteado entre todas as redes conectadas em um só lugar.
    -   **Escalabilidade:** Facilita a expansão da sua arquitetura de rede na nuvem, superando a limitação do roteamento não transitivo do VPC Peering.

-   **Casos de Uso:**
    -   Arquiteturas com um grande número de VPCs (dezenas ou centenas).
    -   Conectar múltiplas redes on-premises a recursos na AWS.
    -   Simplificar a topologia de rede em ambientes multi-contas.

---

## Conectividade Privada com Serviços AWS

### VPC Endpoints

Os **VPC Endpoints** permitem que você conecte sua VPC a serviços da AWS e a serviços de endpoint (VPC endpoint services) de forma **privada**, sem a necessidade de um Internet Gateway, NAT Gateway, conexão VPN ou Direct Connect. O tráfego não sai da rede da Amazon.

-   **Tipos de Endpoints:**
    -   **Gateway Endpoints:** Um gateway que você especifica como um alvo para uma rota em sua tabela de rotas. Suportado apenas para **Amazon S3** e **DynamoDB**.
    -   **Interface Endpoints (AWS PrivateLink):** Utiliza uma ENI (Elastic Network Interface) com um endereço IP privado da sua VPC como ponto de entrada para o tráfego. Suporta a maioria dos outros serviços da AWS e serviços de parceiros.

-   **Benefícios:**
    -   **Segurança Aprimorada:** O tráfego entre sua VPC e o serviço da AWS não sai da rede da Amazon.
    -   **Redução de Custos:** Evita custos de transferência de dados associados ao uso de NAT Gateways para acessar serviços AWS.

-   **Casos de Uso:**
    -   Acessar o S3 ou DynamoDB de instâncias EC2 em sub-redes privadas (Gateway Endpoint).
    -   Conectar-se a serviços como SQS, Kinesis, e API Gateway de forma privada (Interface Endpoint).

---

## Conectividade Híbrida (On-premises para AWS)

### AWS Site-to-Site VPN

A **AWS Site-to-Site VPN** permite que você estabeleça uma conexão segura entre seu data center on-premises ou filial e seus recursos na AWS. A conexão é feita através de um túnel **IPsec** (Internet Protocol Security) criptografado pela **internet pública**.

-   **Componentes Principais:**
    -   **Virtual Private Gateway (VGW) ou Transit Gateway (TGW):** O concentrador de VPN no lado da AWS.
    -   **Customer Gateway (CGW):** Um recurso na AWS que representa o seu dispositivo de VPN no lado on-premises.
    -   **Túnel VPN:** A conexão criptografada.

-   **Casos de Uso:**
    -   Conectar redes corporativas à VPC de forma segura.
    -   Solução de baixo custo e rápida implementação para conectividade híbrida.
    -   Conexão de backup para um AWS Direct Connect.

### AWS Direct Connect

O **AWS Direct Connect** estabelece uma conexão de rede **privada e dedicada** entre sua infraestrutura on-premises e a AWS. Diferente da VPN, ele **não utiliza a internet pública**, oferecendo maior largura de banda, menor latência e uma experiência de rede mais consistente.

-   **Características:**
    -   **Conexões Dedicadas:** Portas de 1 Gbps, 10 Gbps ou 100 Gbps.
    -   **Conexões Hospedadas:** Larguras de banda de 50 Mbps a 10 Gbps, fornecidas por parceiros da AWS.
    -   **Segurança:** A conexão é privada, aumentando a segurança dos dados em trânsito.

-   **Casos de Uso:**
    -   Aplicações que exigem alta largura de banda e baixa latência (críticas ao desempenho).
    -   Transferência de grandes volumes de dados de forma consistente.
    -   Ambientes híbridos que necessitam de uma conexão estável e confiável, evitando a imprevisibilidade da internet.
