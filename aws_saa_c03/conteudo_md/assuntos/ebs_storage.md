|                            Material disponivel                            | estudos concluidos |
|:-------------------------------------------------------------------------:|--------------------|
| [Os Conceitos de Cloud Computing](conceitos_cloud_computing.md) &#x2611;  | &check;            |
|                 [A Amazon AWS](amazon_aws.md)   &#x2611;                  | &check;            |
|                [IAM e Acess Management](iam.md)  &#x2611;                 | &check;            |
|             [ARCHITECT - Armazenamento AWS](s3.md)  &#x2611;              | &check;            |
|              [ARCHITECT - Servidores EC2](ec2.md)  &#x2611;               | &check;            |
|                    [ARCHITECT - VPC](vpc.md)  &#x2611;                    | &check;            |
| [Load Balancers e Auto Scaling](auto_scaling_load_balancers.md) &#x2611;  | &check;            |
|             [ARCHITECT - Database AWS](database.md)  &#x2611;             | &check;            |
|      [DNS, Cache e Performance](dns_cache_performance.md)  &#x2611;       | &check;            |
|          [Block e File Storage](block_file_storage.md)  &#x2610;          | &cross;            |
|         [ARCHITECT - Aplicações AWS](aplicacoes_aws.md)  &#x2610;         | &cross;            |
|                 [Serverless Lambda](lambda.md)  &#x2610;                  | &cross;            |
|           [Segurança na Cloud](seguranca_na_cloud.md)  &#x2610;           | &cross;            |


## Amazon EBS – Tipos de Volumes (Atualizado 2025)

Legenda custo aproximado (comparativo relativo):
- $ barato
- $$ médio
- $$$ alto
- $$$$ muito alto

| Tipo | Classe | Uso Principal | Capacidade (GiB) | IOPS Máx* | Throughput Máx* | Custo (≈) | Observações |
|------|--------|---------------|------------------|-----------|-----------------|-----------|-------------|
| gp3 | SSD Gen. Purpose | Workloads gerais (boot, apps, DB pequenos/médios) | 1 – 16.384 | 16.000 (provisionável) | 1.000 MB/s | $$ | IOPS/throughput desacoplados do tamanho; substituir gp2 |
| gp2 (legado) | SSD Gen. Purpose | Legado (não usar em novos) | 1 – 16.384 | Até 16.000 (depende tamanho) | ~250 MB/s (típico) | $$ | Performance baseada em tamanho (3 IOPS/GB baseline) |
| io2 | SSD Provisioned | Bancos críticos, latência consistente | 4 – 65.536 | 64.000 (Nitro) | 1.000 MB/s | $$$ | Maior durabilidade (99.999%); suporta Multi-Attach |
| io2 Block Express | SSD Provisioned (Extreme) | OLTP de alta escala, SAP HANA, HPC storage-bound | 4 – 64.000 | 256.000 | 4.000 MB/s | $$$$ | Latência microsegundos; requer instâncias compatíveis (Nitro + drivers) |
| st1 | HDD Throughput | Big data sequencial, ETL, logs, streaming | 125 – 16.384 | ~500 IOPS (burst) | 500 MB/s | $ | Performance vinculada ao tamanho (MB/s/terabyte) |
| sc1 | HDD Cold | Dados frios acessados raramente | 125 – 16.384 | ~250 IOPS (burst menor) | 250 MB/s | $ | Menor custo/GB; não para workloads ativos |
| standard (magnetic) | HDD Legacy | Legado para migração | 1 – 1.024 | Baixa | ~40–90 MB/s | $ | Evitar; migrar para gp3 |

*Valores máximos dependem de instância (limites de throughput/IOPS da família EC2 podem ser menores).

### Seleção Rápida
- Alta relação preço/performance geral → gp3.
- Banco de dados de missão crítica com picos intensos → io2.
- Latência extremamente baixa + altíssimo IOPS/throughput → io2 Block Express.
- Processamento analítico sequencial de grandes volumes → st1.
- Arquivo de baixo acesso que precisa ficar montado → sc1.
- Evitar gp2/standard em novos projetos (usar gp3).

### Estratégias de Otimização
| Objetivo | Ação |
|----------|------|
| Reduzir custo mantendo performance moderada | Migrar gp2 → gp3 e ajustar IOPS/throughput para o necessário |
| Evitar degradação inicial após snapshot | Usar Fast Snapshot Restore (FSR) em AZ crítica |
| Crescer volume sem downtime | Elastic Volumes (modify-volume) + ext2/3/4/xfs grow no SO |
| Minimizar latência em DB I/O bound | io2 (ou Block Express) + verificar queue depth e tamanho de página |
| Tolerância a falha + multi writer (cluster) | io1/io2 Multi-Attach (aplicação precisa gerenciar locking) |

### Boas Práticas
- Sempre habilitar criptografia (default em contas novas) – reduz risco de exposição fora do ciclo de vida.
- Fazer planejamento de IOPS vs throughput: IOPS pequenos não garantem throughput alto (tamanho de bloco importa).
- Monitorar métricas CloudWatch: VolumeReadOps, VolumeWriteOps, VolumeQueueLength, BurstBalance (gp2/st1/sc1), VolumeThroughputPercentage.
- Consolidar logs quentes em volumes gp3 dedicados para isolar RU de data volumes.

### Perguntas de Exame (Exemplos)
1. Necessário 200K+ IOPS e 3+ GB/s → io2 Block Express.
2. Workload misto web/app com custo otimizado → gp3 provisionando somente IOPS/throughput necessários.
3. Pipeline ETL leitura sequencial multi-terabyte → st1.
4. Repositório de dados raramente acessado mas precisa montado → sc1.
5. Migrar de gp2 reduzindo custo sem queda de performance → converter para gp3 e calibrar.

### Notas Complementares
- Tamanho mínimo st1/sc1 (125 GiB) força custo mínimo – avaliar S3 Glacier para arquivamento puro.
- io2 Block Express entrega latência single-digit microseconds para parte do caminho I/O (fim-a-fim depende app).
- Para throughput >1.000 MB/s considerar striping (RAID 0) de múltiplos volumes + limites da instância.

---
## Conceitos Fundamentais do EBS

| Conceito | Explicação | Observação de Prova |
|----------|-----------|----------------------|
| Escopo por AZ | Um volume só pode ser anexado a instâncias na mesma AZ | Para mover entre AZ: snapshot + restore |
| Snapshot | Backup incremental armazenado no S3 (gerenciado) | Primeiro snapshot full, demais delta |
| Fast Snapshot Restore (FSR) | Pre-aquece blocos para evitar I/O lento inicial | Custo por hora/AZ enquanto ativo |
| Elastic Volumes | Alterar tipo/tamanho/IOPS em volume em uso | Necessário expandir filesystem no SO |
| Multi-Attach | io1/io2 em múltiplas instâncias simultâneas | Aplicação deve gerenciar locking (cluster FS) |
| Throughput vs IOPS | Throughput = MB/s, IOPS = operações por segundo | Blocos maiores elevam throughput com mesmos IOPS |
| Burst Bucket | gp2/st1/sc1 usam créditos para pico | Monitorar BurstBalance |
| Criptografia | KMS transparente (at-rest + in-transit entre volume/instância) | Sem impacto visível de performance na maioria workloads |

### Métricas Importantes (CloudWatch)
- VolumeReadOps / VolumeWriteOps: volume de operações.
- VolumeQueueLength: filas -> possível gargalo.
- VolumeThroughputPercentage / VolumeConsumedReadWriteOps: saturação.
- BurstBalance (gp2/st1/sc1): saúde de créditos.

---
## Exemplos Práticos por Tipo

### 1. gp3 – Aplicação Web Multi-Camadas
Um e-commerce com instância EC2 rodando backend Java e banco relacional pequeno (<500 GB). Escolhe gp3 500 GiB, provisiona 6.000 IOPS e 500 MB/s (abaixo do máximo) reduzindo custo vs io2. Crescimento esperado? Usa Elastic Volumes futuramente para subir para 1 TB + 8.000 IOPS sem downtime perceptível.

### 2. Migração gp2 → gp3 – Otimização de Custo
Instância tinha volume gp2 1 TB (baseline 3.000 IOPS). Workload precisa apenas 4.000 IOPS sustentados. Migrando para gp3 com 1 TB + 4.000 IOPS + 400 MB/s reduz $/GB e remove dependência de tamanho para performance.

### 3. io2 – Banco OLTP Crítico (PostgreSQL)
Banco de pagamentos 4 TB requer latência consistente e 40K IOPS. Seleciona io2 (provisioned IOPS 40.000). Multi-Attach não necessário. Para failover: replicação na camada de banco (ex. Patroni) para instância standby na mesma AZ ou outra (snapshot restore).

### 4. io2 Block Express – SAP HANA / Alta Densidade
Ambiente SAP HANA 20 TB de dados quentes + exigência >150K IOPS e >2 GB/s. Seleciona io2 Block Express 8 volumes distribuídos (para paralelismo no host) entregando 200K IOPS agregado. Latência microsegundo reduz tempo de resposta de consultas in-memory spillover.

### 5. st1 – Pipeline de Logs e ETL
Cluster de ingestão processa 30 TB de logs por dia sequencialmente. Usa st1 volumes de 4 TB (throughput baseline sobe com tamanho) montados em instâncias de processamento que lêem grandes arquivos para transformação e envio para S3/Redshift. IOPS aleatórios não são críticos.

### 6. sc1 – Repositório de Dados Frios Montado
Relatórios antigos (CSV) precisam ficar acessíveis esporadicamente por analistas sem rehidratar do S3. Volume sc1 2 TB montado em instância utilitária. Acesso pouco frequente; custo mínimo enquanto mantém compatibilidade com scripts legados que esperam caminho de filesystem.

### 7. Multi-Attach (io2) – Sistema de Arquivos Cluster
Dois nós de aplicação precisam acesso simultâneo de leitura/escrita a estruturas de lock compartilhadas. Volume io2 Multi-Attach anexado a ambas instâncias. Aplicação usa cluster-aware filesystem (ex.: OCFS2) para gerenciar locking e coerência.

### 8. Snapshot & FSR – Recuperação Rápida
Procedimento de DR exige restaurar volume de 5 TB e iniciar processamento pesado em <5 minutos sem penalidade de leitura fria. Equipe ativa FSR no snapshot em AZ alvo antes do teste; ao criar volume, desempenho imediato consistente.

### 9. Striping para Throughput
Workload de analytics precisa 2.5 GB/s de leitura em instância que suporta 3 GB/s. Cria 3 volumes gp3 cada 850 GiB, 8.000 IOPS, 800 MB/s e configura RAID 0 (mdadm). Atinge throughput agregado com risco consciente (RAID 0) mitigado via snapshots frequentes.

---
## Checklist de Decisão Rápida
| Pergunta | Escolha Indicada |
|----------|------------------|
| Preciso de latência mais baixa e IOPS altíssimos? | io2 Block Express |
| DB crítico com IOPS estáveis altos mas não extremos? | io2 |
| Workload generalista balanceado custo/perf? | gp3 |
| Processamento sequencial massivo de dados? | st1 |
| Dados montados raramente acessados? | sc1 |
| Várias instâncias escrevendo no mesmo volume? | io2 (Multi-Attach) + FS cluster |
| Necessário reduzir custo de gp2 sem perder IOPS? | Migrar para gp3 |

---
## Erros Comuns em Prova / Produção
- Escolher gp2 em novo design (gp3 mais flexível e econômico).
- Usar st1/sc1 para workload de banco (aleatório) → latência alta.
- Ignorar limite de throughput da instância (volume maior não resolve gargalo de rede/storage host).
- Não considerar Multi-Attach e tentar NFS caseiro sem consistência → corrupção.
- Restaurar snapshot grande sem FSR e estranhar lentidão inicial de leitura.

---
## Resumo Final
Selecione tipo baseado em padrão de I/O (aleatório vs sequencial), necessidade de IOPS/latência, custo alvo e resiliência. Combine Elastic Volumes + gp3 para evolução incremental; reserve io2/Block Express apenas quando métricas justificarem.


