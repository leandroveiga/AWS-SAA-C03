### O que é Amazon VPC (Virtual Private Cloud)

A Amazon Virtual Private Cloud (VPC) é um serviço que permite provisionar uma seção logicamente isolada da nuvem da AWS, onde você pode lançar recursos da AWS em uma rede virtual que você define. Você tem controle total sobre seu ambiente de rede virtual, incluindo a seleção de sua própria faixa de endereços IP, criação de sub-redes e configuração de tabelas de rotas e gateways de rede.

### Como funciona a Amazon VPC

1.  **Criação da VPC:** Você cria uma VPC em uma região da AWS, especificando um bloco de endereços IP CIDR (Classless Inter-Domain Routing), por exemplo, `10.0.0.0/16`. Este é o espaço de IP privado para sua VPC.
2.  **Criação de Sub-redes (Subnets):** Você divide sua VPC em uma ou mais sub-redes. Cada sub-rede reside inteiramente dentro de uma única Zona de Disponibilidade e recebe uma faixa de IP do bloco CIDR da VPC (ex: `10.0.1.0/24`).
    *   **Sub-redes Públicas:** Uma sub-rede é considerada "pública" se tiver uma rota para um Internet Gateway. Instâncias em sub-redes públicas podem ter acesso direto à internet.
    *   **Sub-redes Privadas:** Uma sub-rede que não tem uma rota para um Internet Gateway. Instâncias em sub-redes privadas não podem ser acessadas diretamente da internet.
3.  **Configuração de Roteamento e Gateways:**
    *   **Tabelas de Rotas (Route Tables):** Controlam para onde o tráfego de rede de sua sub-rede é direcionado. Cada sub-rede deve ser associada a uma tabela de rotas.
    *   **Internet Gateway (IGW):** Um componente horizontalmente escalável, redundante e altamente disponível que permite a comunicação entre sua VPC e a internet.
    *   **NAT Gateway (ou Instância NAT):** Permite que instâncias em uma sub-rede privada iniciem conexões de saída para a internet (por exemplo, para atualizações de software), mas impede que conexões de entrada sejam iniciadas da internet.
4.  **Segurança:**
    *   **Network Access Control Lists (NACLs):** Um firewall opcional no nível da sub-rede para controlar o tráfego de entrada e saída. São "stateless", o que significa que as regras de entrada e saída devem ser definidas explicitamente.
    *   **Security Groups:** Um firewall no nível da instância (ou ENI) para controlar o tráfego. São "stateful", o que significa que se você permitir o tráfego de entrada, o tráfego de retorno correspondente é automaticamente permitido.

### Componentes Principais

*   **VPC:** Sua rede virtual privada na nuvem.
*   **Subnet:** Uma subdivisão de sua VPC.
*   **Route Table:** Um conjunto de regras que determina para onde o tráfego de rede é direcionado.
*   **Internet Gateway:** Permite o acesso à internet.
*   **NAT Gateway:** Permite que instâncias privadas acessem a internet.
*   **Security Groups:** Firewall no nível da instância.
*   **NACLs:** Firewall no nível da sub-rede.
*   **VPC Peering:** Permite conectar duas VPCs para que possam se comunicar como se estivessem na mesma rede.
*   **VPC Endpoints:** Permitem conectar-se privadamente a serviços da AWS suportados sem exigir um Internet Gateway ou NAT Gateway.

### Benefícios da VPC

*   **Isolamento e Segurança:** Fornece um ambiente de rede seguro e isolado para seus recursos.
*   **Controle Total:** Você tem controle granular sobre sua rede, incluindo endereçamento IP, roteamento e regras de firewall.
*   **Conectividade Híbrida:** Permite conectar sua VPC ao seu próprio data center corporativo usando uma VPN ou AWS Direct Connect.
*   **Flexibilidade:** Permite projetar topologias de rede complexas que atendam às suas necessidades específicas.
