### O que é Amazon EBS (Elastic Block Store)

O Amazon Elastic Block Store (EBS) fornece volumes de armazenamento em nível de bloco de alto desempenho para uso com instâncias Amazon EC2. É semelhante a um disco rígido de rede que você pode anexar a uma única instância EC2. Uma vez anexado, você pode formatá-lo com um sistema de arquivos e usá-lo como um disco local para instalar um sistema operacional, executar um banco de dados ou armazenar dados de aplicativos.

### Como funciona o Amazon EBS

1.  **Criação de um Volume:** Você cria um volume EBS em uma Zona de Disponibilidade específica, definindo seu tamanho e tipo de desempenho.
2.  **Anexação a uma Instância:** Você anexa o volume a uma instância EC2 que esteja na mesma Zona de Disponibilidade. O volume aparece para o sistema operacional como um dispositivo de bloco bruto.
3.  **Uso como Disco Local:** Você pode formatar o volume com qualquer sistema de arquivos (como ext4 ou NTFS) e montá-lo. A partir desse ponto, ele se comporta como um disco local.
4.  **Persistência de Dados:** Os dados em um volume EBS persistem independentemente da vida útil da instância EC2. Você pode desanexar o volume de uma instância e anexá-lo a outra.
5.  **Snapshots:** Você pode criar backups pontuais de seus volumes EBS na forma de "snapshots", que são armazenados de forma incremental no Amazon S3. Snapshots podem ser usados para restaurar volumes ou criar novos volumes idênticos.

### Tipos de Volume EBS

*   **General Purpose SSD (gp2/gp3):** Equilibram preço e desempenho para uma ampla variedade de cargas de trabalho, como volumes de inicialização, desenvolvimento e teste. `gp3` é a geração mais recente e mais econômica.
*   **Provisioned IOPS SSD (io1/io2):** Projetados para cargas de trabalho intensivas em I/O e com uso intensivo de banco de dados, que exigem desempenho de IOPS consistente e alto. `io2 Block Express` oferece a maior performance.
*   **Throughput Optimized HDD (st1):** Discos rígidos de baixo custo projetados para cargas de trabalho com alto throughput e acesso frequente, como big data e data warehouses.
*   **Cold HDD (sc1):** O tipo de armazenamento HDD de menor custo, projetado para dados acessados com pouca frequência.

### Benefícios do EBS

*   **Alto Desempenho:** Oferece vários tipos de volume otimizados para diferentes necessidades de desempenho, incluindo SSDs de baixa latência.
*   **Persistência:** Os dados são mantidos mesmo que a instância EC2 seja interrompida ou encerrada.
*   **Disponibilidade e Durabilidade:** Os volumes são replicados automaticamente dentro de sua Zona de Disponibilidade para proteção contra falhas de componentes.
*   **Flexibilidade:** Permite modificar o tamanho e o tipo do volume dinamicamente, sem tempo de inatividade.
*   **Backup e Recuperação:** Snapshots facilitam a criação de backups e a recuperação de dados.
