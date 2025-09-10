### O que é AWS Storage Gateway

O AWS Storage Gateway é um serviço de armazenamento em nuvem híbrido que permite que suas aplicações on-premises (locais) usem o armazenamento da AWS de forma transparente. Ele conecta seu ambiente de software local a uma infraestrutura de armazenamento baseada em nuvem, fornecendo uma ponte segura e de baixa latência entre seus data centers e a nuvem da AWS.

### Como funciona o AWS Storage Gateway

1.  **Implantação do Appliance:** Você implanta um appliance virtual (VMware ESXi, Microsoft Hyper-V) ou um appliance de hardware dedicado em seu data center local.
2.  **Conexão com a AWS:** O appliance se conecta de forma segura aos serviços de armazenamento da AWS, como Amazon S3, Amazon EBS e Amazon Glacier.
3.  **Apresentação de Interfaces Padrão:** O gateway apresenta interfaces de armazenamento padrão (NFS, SMB, iSCSI) para suas aplicações locais. Isso significa que suas aplicações podem acessar o armazenamento da AWS como se fossem dispositivos de armazenamento locais.
4.  **Cache Local:** O gateway mantém um cache local dos dados acessados com mais frequência, proporcionando acesso de baixa latência para suas aplicações, enquanto armazena de forma durável os dados completos na nuvem da AWS.

### Tipos de Gateway

#### 1. File Gateway (Gateway de Arquivos)
*   **O que faz:** Fornece uma interface de compartilhamento de arquivos (NFS e SMB) para objetos no Amazon S3. Os arquivos são armazenados como objetos no S3, e você pode acessá-los diretamente na nuvem ou através do gateway.
*   **Como funciona:** Apresenta um ponto de montagem de arquivo para seus servidores locais. Quando suas aplicações gravam arquivos no compartilhamento, o gateway os carrega para o S3.
*   **Casos de uso:** Migração de compartilhamentos de arquivos para a nuvem, backup de dados locais no S3 e fornecimento de acesso de baixa latência a dados na nuvem para aplicações on-premises.

#### 2. Volume Gateway (Gateway de Volumes)
Apresenta volumes de armazenamento em bloco iSCSI para suas aplicações.

*   **Modo de Volumes Armazenados (Stored Volumes):** Armazena todos os seus dados localmente no seu data center e faz backup assíncrono desses dados como snapshots do EBS na AWS. Fornece acesso de baixa latência a todo o conjunto de dados.
    *   **Casos de uso:** Backup local e recuperação de desastres rápida.
*   **Modo de Volumes em Cache (Cached Volumes):** Armazena todos os seus dados primários no Amazon S3, mas mantém os dados acessados com mais frequência em cache localmente no gateway.
    *   **Casos de uso:** Expansão do armazenamento local para a nuvem, economizando custos com armazenamento primário.

#### 3. Tape Gateway (Gateway de Fitas)
*   **O que faz:** Apresenta uma biblioteca de fitas virtuais (VTL) para seu software de backup local. Ele substitui o uso de fitas físicas por fitas virtuais armazenadas na AWS.
*   **Como funciona:** Seu software de backup se integra com a VTL via iSCSI. Os backups são armazenados em fitas virtuais no S3, e para arquivamento de longo prazo, as fitas podem ser movidas para o S3 Glacier Deep Archive.
*   **Casos de uso:** Modernização de fluxos de trabalho de backup em fita, eliminando a necessidade de infraestrutura de fita física.

### Benefícios do Storage Gateway

*   **Integração Transparente:** Permite que aplicações existentes acessem o armazenamento da AWS sem modificações.
*   **Desempenho de Baixa Latência:** O cache local garante que os dados ativos sejam acessados rapidamente.
*   **Transferência Otimizada e Segura:** Compacta e criptografa os dados em trânsito para a AWS.
*   **Durabilidade e Escalabilidade:** Aproveita a durabilidade e a escala praticamente ilimitada do armazenamento da AWS.
