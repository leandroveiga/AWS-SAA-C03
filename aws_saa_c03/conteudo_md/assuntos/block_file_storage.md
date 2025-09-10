# Armazenamento em Bloco e Arquivo na AWS

Além do armazenamento de objetos (S3), a AWS oferece soluções de armazenamento em bloco e em arquivo, cada uma projetada para casos de uso específicos, principalmente para uso com instâncias EC2.

## 1. Amazon EBS (Elastic Block Store)

O EBS fornece volumes de armazenamento em **bloco** de alta performance para uso com instâncias Amazon EC2. Pense no EBS como um disco rígido externo de rede que você anexa a uma única instância EC2.

-   **Conceito Principal:** Um volume EBS é vinculado a uma **única instância EC2** em uma **única Zona de Disponibilidade (AZ)**. Para mover um volume para outra AZ, você precisa criar um snapshot e restaurá-lo na nova AZ.
-   **Snapshots:** São backups incrementais dos seus volumes EBS, armazenados de forma econômica no Amazon S3. Você pode usar snapshots para criar novos volumes, expandir volumes ou restaurar dados.
-   **Tipos de Volume:**
    -   **SSD (Solid State Drives):** Otimizados para cargas de trabalho transacionais com IOPS (operações de I/O por segundo) altas.
        -   **gp2/gp3 (General Purpose SSD):** Equilibram preço e performance para uma ampla variedade de cargas de trabalho (ex: volumes de inicialização, bancos de dados de pequeno e médio porte). **gp3** é a geração mais recente e econômica.
        -   **io1/io2 (Provisioned IOPS SSD):** Para aplicações críticas de negócios que exigem performance de IOPS sustentada, como grandes bancos de dados relacionais e NoSQL. **io2 Block Express** oferece a mais alta performance.
    -   **HDD (Hard Disk Drives):** Otimizados para cargas de trabalho com alta taxa de transferência (throughput - MB/s).
        -   **st1 (Throughput Optimized HDD):** Para dados acessados com frequência que exigem alta taxa de transferência, como big data e data warehouses.
        -   **sc1 (Cold HDD):** O custo mais baixo, para dados acessados com pouca frequência.

## 2. Amazon EFS (Elastic File System)

O EFS fornece um sistema de arquivos de **arquivo** simples, escalável e elástico para uso com instâncias EC2 e outros serviços da AWS.

-   **Conceito Principal:** O EFS é um sistema de arquivos compartilhado. Ele pode ser montado e acessado por **múltiplas instâncias EC2 simultaneamente**, inclusive em **diferentes Zonas de Disponibilidade** dentro da mesma região.
-   **Escalabilidade:** É "elástico", o que significa que ele cresce e diminui automaticamente à medida que você adiciona e remove arquivos, e você paga apenas pelo armazenamento que usa.
-   **Protocolo:** Usa o protocolo NFS (Network File System).
-   **Classes de Armazenamento:**
    -   **EFS Standard:** Para dados acessados com frequência.
    -   **EFS Infrequent Access (EFS-IA):** Uma classe de armazenamento de baixo custo para arquivos acessados com pouca frequência. O EFS pode mover arquivos automaticamente para o EFS-IA com base em políticas de ciclo de vida.
-   **Caso de Uso:** Ideal para gerenciamento de conteúdo, home directories, e qualquer aplicação que precise de um sistema de arquivos compartilhado.

## 3. Amazon FSx (File System Extra)

O Amazon FSx é um serviço que permite lançar, executar e escalar sistemas de arquivos de terceiros, ricos em recursos e de alta performance.

-   **Amazon FSx for Windows File Server:**
    -   **O que é:** Fornece um sistema de arquivos totalmente gerenciado, nativo do Microsoft Windows, construído sobre o Windows Server.
    -   **Protocolo:** Suporta o protocolo SMB (Server Message Block).
    -   **Caso de Uso:** Perfeito para migrar aplicações Windows que precisam de armazenamento de arquivos compartilhado para a AWS. Integra-se nativamente com o Microsoft Active Directory.

-   **Amazon FSx for Lustre:**
    -   **O que é:** Um sistema de arquivos de alta performance otimizado para cargas de trabalho de computação de alto desempenho (HPC), como machine learning e análise de big data.
    -   **Integração:** Pode ser vinculado ao Amazon S3. Você pode processar seus dados do S3 em alta velocidade com o FSx for Lustre e gravar os resultados de volta no S3.

## Comparativo Rápido: EBS vs. EFS vs. FSx

| Característica | EBS (Elastic Block Store) | EFS (Elastic File System) | FSx for Windows |
| :--- | :--- | :--- | :--- |
| **Tipo** | Armazenamento em **Bloco** | Armazenamento de **Arquivo** | Armazenamento de **Arquivo** |
| **Acesso** | Acessado por **1** instância EC2 em **1** AZ | Acessado por **múltiplas** instâncias EC2 em **múltiplas** AZs | Acessado por múltiplas instâncias (foco em Windows) |
| **Protocolo** | N/A (Aparece como um disco local) | NFS | SMB |
| **Principal Caso de Uso** | Disco de boot, bancos de dados, armazenamento para uma única instância. | Armazenamento compartilhado para aplicações Linux, CMS, home directories. | Armazenamento compartilhado para aplicações Windows, "lift-and-shift" de servidores de arquivos Windows. |

