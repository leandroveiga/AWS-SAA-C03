### O que é Amazon EFS (Elastic File System)

O Amazon Elastic File System (EFS) é um serviço de armazenamento de arquivos simples, escalável e totalmente gerenciado para uso com instâncias Amazon EC2 e serviços de contêiner da AWS. Ele fornece um sistema de arquivos compartilhado que pode ser montado em várias instâncias EC2 simultaneamente. O EFS foi projetado para escalar sob demanda para petabytes sem interromper as aplicações, crescendo e diminuindo automaticamente à medida que você adiciona e remove arquivos.

### Como funciona o Amazon EFS

1.  **Criação de um Sistema de Arquivos:** Você cria um sistema de arquivos EFS em sua VPC. O serviço cria automaticamente "Mount Targets" (pontos de montagem de rede) em cada Zona de Disponibilidade que você especificar.
2.  **Montagem em Instâncias:** A partir de suas instâncias EC2 (Linux), você usa o comando de montagem padrão do NFS (Network File System) para montar o sistema de arquivos EFS usando o DNS do Mount Target.
3.  **Acesso Compartilhado:** Uma vez montado, várias instâncias EC2, mesmo em diferentes Zonas de Disponibilidade, podem acessar o sistema de arquivos EFS ao mesmo tempo. As alterações feitas por uma instância são imediatamente visíveis para as outras.
4.  **Escalonamento Automático:** O armazenamento é elástico. Ele cresce e diminui automaticamente conforme você adiciona ou remove arquivos, e você paga apenas pelo espaço que utiliza.
5.  **Consistência de Dados:** O EFS fornece consistência de "leitura após escrita" (read-after-write), garantindo que, uma vez que um arquivo seja gravado com sucesso, qualquer leitura subsequente receberá os dados mais recentes.

### Classes de Armazenamento e Ciclo de Vida

*   **EFS Standard:** Para dados acessados com frequência.
*   **EFS Infrequent Access (EFS IA):** Uma classe de armazenamento de custo mais baixo para arquivos que são acessados com menos frequência.
*   **Gerenciamento do Ciclo de Vida:** Você pode configurar uma política para mover automaticamente arquivos que não são acessados por um determinado período (por exemplo, 30 dias) da classe EFS Standard para a EFS IA, otimizando custos.

### Benefícios do EFS

*   **Armazenamento Compartilhado:** Permite que centenas ou milhares de instâncias EC2 acessem o mesmo sistema de arquivos simultaneamente.
*   **Totalmente Gerenciado e Elástico:** Não há necessidade de provisionar ou gerenciar capacidade de armazenamento. Ele escala de gigabytes a petabytes automaticamente.
*   **Alta Disponibilidade e Durabilidade:** Os dados são armazenados de forma redundante em várias Zonas de Disponibilidade.
*   **Baseado em Padrões:** Usa o protocolo NFSv4, o que o torna compatível com as aplicações e ferramentas Linux existentes.
*   **Custo-Benefício:** Pague apenas pelo armazenamento que você usa, com opções de classes de armazenamento para otimizar ainda mais os custos.
