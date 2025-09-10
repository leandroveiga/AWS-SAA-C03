### O que é Amazon FSx

O Amazon FSx é um serviço totalmente gerenciado que facilita o lançamento e a execução de sistemas de arquivos de terceiros populares e de alto desempenho. Ele fornece sistemas de arquivos ricos em recursos e altamente performáticos, lidando com o gerenciamento de hardware, software, patches e backups. O Amazon FSx oferece diferentes opções, cada uma otimizada para cargas de trabalho específicas.

### Tipos de Amazon FSx

#### 1. Amazon FSx for Windows File Server
Fornece um sistema de arquivos Microsoft Windows totalmente gerenciado, construído sobre o Windows Server. Ele suporta o protocolo SMB (Server Message Block) e recursos como Active Directory (AD), ACLs do Windows e DFS (Distributed File System).

*   **Como funciona:** Você cria um sistema de arquivos e o associa ao seu Microsoft Active Directory (AWS Managed ou auto-gerenciado). As instâncias do Windows podem então mapear o compartilhamento de arquivos como uma unidade de rede padrão.
*   **Casos de uso:** Aplicações .NET, diretórios de base de usuários (home directories) e outras cargas de trabalho baseadas no Windows que precisam de armazenamento de arquivos compartilhado.

#### 2. Amazon FSx for Lustre
Fornece um sistema de arquivos de alto desempenho otimizado para cargas de trabalho de computação rápida, como computação de alto desempenho (HPC), machine learning e fluxos de trabalho de mídia. O Lustre é um sistema de arquivos paralelos popular que pode escalar para centenas de gigabytes por segundo de throughput e milhões de IOPS.

*   **Como funciona:** Você cria um sistema de arquivos FSx for Lustre e pode vinculá-lo a um bucket do Amazon S3. Isso permite processar dados do S3 em alta velocidade e gravar os resultados de volta no S3.
*   **Casos de uso:** Análise de dados, processamento de vídeo, simulações financeiras e qualquer aplicação que precise de processamento massivamente paralelo em grandes conjuntos de dados.

#### 3. Amazon FSx for NetApp ONTAP
Oferece o popular sistema de arquivos ONTAP da NetApp como um serviço totalmente gerenciado na AWS. Ele fornece os recursos, o desempenho e as APIs de gerenciamento de dados do ONTAP com a agilidade e escalabilidade da nuvem.

*   **Como funciona:** Permite que você migre e execute suas aplicações que dependem de armazenamento NAS da NetApp sem precisar alterar o código ou a forma como você gerencia os dados.
*   **Casos de uso:** Migração de cargas de trabalho que usam ONTAP on-premises, recuperação de desastres, e execução de bancos de dados e aplicações empresariais.

### Benefícios do Amazon FSx

*   **Totalmente Gerenciado:** A AWS gerencia o hardware e o software, automatizando tarefas como provisionamento, patching e backups.
*   **Alto Desempenho:** Cada tipo de FSx é otimizado para fornecer o alto desempenho exigido por suas respectivas cargas de trabalho.
*   **Rico em Recursos e Compatível:** Fornece os recursos e protocolos familiares dos sistemas de arquivos que você já usa (SMB, NFS, Lustre).
*   **Segurança:** Criptografa dados em repouso e em trânsito e se integra com serviços de identidade como o Active Directory.
