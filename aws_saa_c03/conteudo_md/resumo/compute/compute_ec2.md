### O que é Amazon EC2 (Elastic Compute Cloud)

O Amazon Elastic Compute Cloud (EC2) é um serviço da web que fornece capacidade computacional segura e redimensionável na nuvem. Ele foi projetado para facilitar a computação em escala da web para os desenvolvedores. O EC2 permite que você obtenha e configure capacidade com o mínimo de atrito, oferecendo controle total sobre seus recursos de computação. Essencialmente, são servidores virtuais (conhecidos como instâncias) na nuvem da AWS.

### Como funciona o Amazon EC2

1.  **Escolha de uma Imagem (AMI):** Você começa selecionando uma Amazon Machine Image (AMI), que é um modelo pré-configurado contendo o sistema operacional e softwares básicos necessários para iniciar sua instância.
2.  **Seleção do Tipo de Instância:** Você escolhe um tipo de instância, que define a configuração de hardware (CPU, memória, armazenamento e capacidade de rede) do seu servidor virtual. Existem tipos otimizados para computação, memória, armazenamento, etc.
3.  **Configuração da Instância:** Você configura detalhes como rede (VPC e sub-rede), armazenamento (volumes EBS) e segurança (Security Groups, que atuam como um firewall virtual).
4.  **Lançamento e Acesso:** A instância é lançada na região da AWS especificada. Após o lançamento, você pode se conectar a ela (por exemplo, via SSH para Linux ou RDP para Windows) e usá-la como faria com um servidor físico.
5.  **Gerenciamento:** Você pode parar, iniciar, reiniciar ou terminar a instância a qualquer momento. O faturamento pode ser por segundo, com diferentes modelos de preço (On-Demand, Spot, Savings Plans).

### Componentes Principais

*   **Instâncias:** Os servidores virtuais.
*   **Amazon Machine Images (AMIs):** Modelos para a criação de instâncias.
*   **Tipos de Instância:** Várias configurações de CPU, memória, armazenamento e rede.
*   **Amazon EBS (Elastic Block Store):** Volumes de armazenamento em bloco persistentes para uso com instâncias EC2.
*   **Security Groups:** Firewall virtual no nível da instância para controlar o tráfego de entrada e saída.
*   **Pares de Chaves:** Credenciais seguras (chave pública/privada) usadas para provar sua identidade ao se conectar a uma instância.

### Benefícios do EC2

*   **Elasticidade:** Permite aumentar ou diminuir a capacidade em minutos, não em horas ou dias.
*   **Controle:** Você tem acesso root e controle total sobre suas instâncias.
*   **Custo-Benefício:** Oferece diversos modelos de preços (On-Demand, Spot, etc.) para otimizar os custos com base no seu caso de uso.
*   **Flexibilidade:** Ampla seleção de sistemas operacionais, softwares e tipos de instância.
*   **Integração:** Totalmente integrado com outros serviços da AWS, como S3, RDS e VPC.
