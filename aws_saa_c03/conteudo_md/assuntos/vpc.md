# Amazon VPC (Virtual Private Cloud)

A **Amazon VPC** permite que você provisione uma seção da Nuvem AWS isolada logicamente, onde é possível executar recursos da AWS em uma rede virtual que você define. Você tem controle total sobre seu ambiente de rede virtual, incluindo a seleção de seu próprio intervalo de endereços IP, a criação de sub-redes e a configuração de tabelas de rotas e gateways de rede.

---

## Componentes Fundamentais da VPC

-   **VPC:** O contêiner lógico para sua rede isolada. É definido em uma única Região e pode abranger múltiplas Zonas de Disponibilidade.
-   **Sub-redes (Subnets):** Um intervalo de endereços IP em sua VPC. Uma sub-rede deve residir inteiramente em uma única Zona de Disponibilidade.
    -   **Sub-rede Pública:** Uma sub-rede cuja tabela de rotas tem uma rota para um **Internet Gateway**. Instâncias aqui podem ter acesso direto à internet.
    -   **Sub-rede Privada:** Uma sub-rede que não tem uma rota para um Internet Gateway. Instâncias aqui não podem ser acessadas diretamente da internet.
-   **Tabelas de Rota (Route Tables):** Um conjunto de regras, chamadas de rotas, que são usadas para determinar para onde o tráfego de rede de sua sub-rede ou gateway é direcionado.
-   **Internet Gateway (IGW):** Um componente de VPC escalável horizontalmente, redundante e altamente disponível que permite a comunicação entre sua VPC e a internet.
-   **NAT Gateway (Network Address Translation):** Permite que instâncias em uma sub-rede privada se conectem a serviços fora da sua VPC (como a internet), mas impede que serviços externos iniciem uma conexão com essas instâncias. É um serviço gerenciado e altamente disponível da AWS.
-   **Bastion Host (ou Jump Box):** Uma instância EC2 localizada em uma sub-rede pública que é usada para se conectar de forma segura (via SSH ou RDP) a instâncias em sub-redes privadas.

---

## Segurança na VPC

A segurança da VPC é controlada em duas camadas:

### 1. Network Access Control Lists (NACLs)
-   **Nível de Operação:** Sub-rede. Atua como um firewall para controlar o tráfego de entrada e saída da sub-rede.
-   **Tipo:** *Stateless* (Sem estado). Isso significa que as regras de entrada e saída são avaliadas separadamente. Se você permite tráfego de entrada na porta 80, precisa permitir explicitamente o tráfego de saída correspondente (nas portas efêmeras 1024-65535).
-   **Regras:** Suporta regras de `allow` (permitir) e `deny` (negar). As regras são avaliadas em ordem numérica.

### 2. Security Groups (Grupos de Segurança)
-   **Nível de Operação:** Instância (especificamente, na interface de rede - ENI). Atua como um firewall para a instância.
-   **Tipo:** *Stateful* (Com estado). Se você permite tráfego de entrada, o tráfego de saída correspondente é automaticamente permitido, e vice-versa.
-   **Regras:** Suporta apenas regras de `allow`. Tudo o que não é explicitamente permitido é negado por padrão.

| Característica | Security Group | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Escopo** | Nível da Instância (ENI) | Nível da Sub-rede |
| **Estado** | Stateful | Stateless |
| **Regras** | Apenas `Allow` | `Allow` e `Deny` |
| **Avaliação** | Todas as regras são avaliadas | Regras avaliadas em ordem numérica |

---

## Conectividade da VPC

### Conectando VPCs entre si:
-   **VPC Peering:** Permite conectar duas VPCs para que elas possam se comunicar como se estivessem na mesma rede. A conexão é privada, usando a infraestrutura da AWS. Não suporta roteamento transitivo (se A está conectada a B, e B a C, A não pode falar com C através de B).
-   **AWS Transit Gateway:** Atua como um hub central para conectar suas VPCs e redes on-premises. Simplifica a topologia de rede e resolve o problema do roteamento transitivo. É a solução moderna para conectar múltiplas VPCs.

### Conectando sua Rede On-Premises à VPC:
-   **AWS Site-to-Site VPN:** Cria uma conexão segura e criptografada (um túnel IPsec) entre seu data center e sua VPC pela internet pública.
-   **AWS Direct Connect (DX):** Estabelece uma conexão de rede privada e dedicada entre sua rede e um dos locais do Direct Connect. Oferece largura de banda consistente e menor latência em comparação com uma VPN baseada na internet.
    -   **Direct Connect Gateway:** Permite usar uma única conexão Direct Connect para se conectar a todas as suas VPCs em qualquer região (exceto China).

### Acesso Privado a Serviços AWS:
-   **VPC Endpoints:** Permitem que você se conecte de forma privada e segura a serviços da AWS (como S3, DynamoDB, etc.) sem exigir um Internet Gateway, NAT Gateway, conexão VPN ou Direct Connect. O tráfego entre sua VPC e o serviço da AWS não sai da rede da Amazon.
    -   **Gateway Endpoints:** Um gateway que você especifica como um alvo para uma rota em sua tabela de rotas. Suporta S3 e DynamoDB.
    -   **Interface Endpoints (AWS PrivateLink):** Uma interface de rede elástica (ENI) com um endereço IP privado que serve como ponto de entrada para o tráfego destinado a um serviço suportado. Suporta a maioria dos serviços da AWS.

