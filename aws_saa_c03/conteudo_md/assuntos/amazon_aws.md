# A Infraestrutura Global da AWS

A infraestrutura da AWS é projetada para ser altamente disponível, resiliente e escalável. Ela é composta por vários componentes hierárquicos.

## Regiões (Regions)
<img src="../../img/regiao.png">

-   **O que são:** Uma Região é uma área geográfica física no mundo onde a AWS possui múltiplos data centers. Exemplos: `us-east-1` (Norte da Virgínia), `sa-east-1` (São Paulo).
-   **Isolamento:** As Regiões são completamente isoladas umas das outras. Isso garante a maior tolerância a falhas e estabilidade.
-   **Escolha da Região:** A escolha de uma região é uma decisão crítica e geralmente baseada em:
    1.  **Latência:** Ficar mais perto dos seus usuários para reduzir o tempo de resposta.
    2.  **Custo:** Os preços dos serviços variam entre as regiões.
    3.  **Conformidade (Compliance):** Requisitos legais e de soberania de dados (ex: GDPR na Europa).
    4.  **Disponibilidade de Serviços:** Nem todos os serviços da AWS estão disponíveis em todas as regiões.

## Zonas de Disponibilidade (Availability Zones - AZs)
<img src="../../img/azs.png">

-   **O que são:** Dentro de cada Região, existem múltiplas Zonas de Disponibilidade. Cada AZ é um ou mais data centers discretos com energia, refrigeração e rede redundantes.
-   **Isolamento:** As AZs são fisicamente separadas por uma distância significativa (quilômetros) para evitar que um desastre em uma afete as outras, mas próximas o suficiente para terem baixa latência (<10ms) entre elas.
-   **Alta Disponibilidade:** O conceito fundamental é distribuir suas aplicações em **múltiplas AZs** para garantir alta disponibilidade. Se uma AZ falhar, sua aplicação continua funcionando nas outras.

## Pontos de Presença (Edge Locations) e AWS Global Network
<img src="../../img/rede.png">

-   **O que são:** São locais onde a AWS armazena em cache cópias do seu conteúdo (via **Amazon CloudFront**) para que ele possa ser entregue mais rapidamente aos usuários em todo o mundo.
-   **Diferença para AZs:** Existem muito mais Pontos de Presença do que AZs. Eles são usados para entregar conteúdo com baixa latência, não para executar sua infraestrutura principal como o EC2.
-   **Rede Global:** Toda a infraestrutura da AWS (Regiões, AZs, Edge Locations) é interconectada por uma rede de fibra óptica global, privada e de alta velocidade, que a AWS controla.

## Extensões da Infraestrutura AWS

### AWS Local Zones
<img src="../../img/local_zones.png">

-   **O que são:** Uma extensão de uma Região da AWS que coloca computação, armazenamento e outros serviços selecionados mais perto de grandes centros populacionais, industriais e de TI.
-   **Caso de Uso:** Aplicações que exigem latência de um dígito de milissegundo para os usuários finais ou instalações on-premises em uma cidade específica.

### AWS Wavelength
<img src="../../img/wavelength.jpg">

-   **O que são:** Incorpora serviços de computação e armazenamento da AWS na borda das redes 5G das operadoras de telecomunicações.
-   **Caso de Uso:** Aplicações de latência ultrabaixa para dispositivos móveis, como streaming de jogos, realidade virtual e carros conectados.

### AWS Outposts
<img src="../../img/outpost.png">

-   **O que é:** Um serviço totalmente gerenciado que estende a infraestrutura, os serviços, as APIs e as ferramentas da AWS para praticamente qualquer data center, espaço de co-location ou instalação on-premises do cliente.
-   **Caso de Uso:** Cargas de trabalho que precisam permanecer on-premises devido a requisitos de baixa latência, processamento de dados local ou residência de dados. Essencialmente, é "trazer a AWS para o seu data center".


