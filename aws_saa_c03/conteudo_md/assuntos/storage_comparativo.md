# Comparativo EBS vs EFS vs FSx (2025)

Tabela e heurísticas para seleção rápida em arquitetura de soluções (Exame SAA-C03):

## Visão Geral
| Aspecto | EBS | EFS | FSx (Windows / Lustre / ONTAP / OpenZFS) |
|---------|-----|-----|-----------------------------------------|
| Tipo | Block | File (NFS) | File (SMB / NFS / iSCSI / HPC) |
| Escopo | 1 instância (ou Multi-Attach io1/io2) | Multi instâncias Linux | Multi instâncias (depende flavor) |
| Elasticidade Tamanho | Manual (Elastic Volumes) | Automático | Provisionado (varia) |
| Latência | Baixíssima (SSD) | Baixa a moderada | Varia (Windows moderada, Lustre alta perf sequencial, ONTAP/OpenZFS baixa) |
| Protocolos | Block device | NFSv4 | SMB, NFS, iSCSI (ONTAP), POSIX HPC (Lustre) |
| Multi-AZ | Snapshot restore / replicação app | Standard multi-AZ | Windows Multi-AZ, ONTAP HA; Lustre (principalmente single-AZ); OpenZFS single-AZ |
| Performance Escalável | Por tipo/IOPS/striping | Pelo tamanho (burst) ou throughput provisionado/elastic | Por flavor (Lustre escala massivo; ONTAP otimiza com cache/dedupe) |
| Uso Típico | OS, DB, logs, aplicações estado local | Conteúdo compartilhado, config, home dirs | Windows shares, HPC, enterprise storage, builds |
| Preço (alto nível) | $–$$$$ (gp3→io2BX) | $$–$$$ | $$–$$$$ |
| Criptografia | KMS | KMS | KMS |
| Backup | Snapshots | AWS Backup | AWS Backup / snapshots específicos |

## Custos Relativos (Resumo)
| Serviço | Faixa Geral |
|---------|-------------|
| EBS gp3 | $$ |
| EBS io2 Block Express | $$$$ |
| EFS Standard | $$$ |
| EFS One Zone-IA | $ |
| FSx Windows | $$$ |
| FSx Lustre (alta perf) | $$$$ |
| FSx ONTAP | $$$ |
| FSx OpenZFS | $$ |

## Escolha Rápida (Fluxo Mental)
1. Precisa de filesystem POSIX compartilhado simples e elástico? → EFS.
2. Precisa de bloco de alta performance acoplado a instância (DB) → EBS (io2 / gp3).
3. Workload Windows com SMB e AD? → FSx for Windows.
4. HPC / ML com dataset grande em S3 → FSx for Lustre.
5. Multi-protocolo (NFS + SMB) + snapshots/replicação enterprise → FSx for ONTAP.
6. Latência baixa para builds CI com snapshots leves → FSx OpenZFS.
7. Custo mínimo para dados frios montados ainda acessíveis → EFS One Zone-IA (ou repensar S3 + sync).

## Exemplos de Questões
| Pergunta | Melhor Serviço | Justificativa |
|----------|----------------|---------------|
| Banco relacional crítico precisa 60K IOPS | EBS io2 | Provisioned IOPS consistente |
| Container fleet precisa compartilhar libs e configs dinâmicas | EFS Standard | NFS multi-AZ elástico |
| Render farm precisa ler dataset de texturas de alta taxa | FSx Lustre | Throughput paralelizado alto |
| Legacy .NET app precisa ACL NTFS | FSx Windows | Suporte SMB + AD |
| Ambiente híbrido replicando de NetApp on-prem | FSx ONTAP | SnapMirror + multi-protocolo |

## Armadilhas Comuns
- Usar EFS para banco de dados relacional de alta IOPS (latência e consistência inadequadas) – usar EBS.
- Tentar compartilhar EBS padrão entre várias instâncias (não suportado; Multi-Attach só io1/io2 e exige app cluster FS).
- Escolher Lustre para workload que não precisa de throughput extremo (custo maior sem necessidade).
- Ignorar classes IA / One Zone em EFS para dados frios e pagar mais.

## Integração com S3
| Serviço | Integração | Uso |
|---------|------------|-----|
| EBS | Via snapshots (armazenados em S3 internamente) | Backup / clone / AMI |
| EFS | Não nativa (scripts / DataSync) | Arquivamento / sync |
| FSx Lustre | Nativa (data repository) | Import automática e export pós-processamento |
| FSx ONTAP | FabricPool (tiering para S3) | Tier dados frios |
| FSx Windows / OpenZFS | Via DataSync / scripts | Migração / backup |

## Segurança / Controle de Acesso
| Serviço | Mecanismo Principal |
|---------|--------------------|
| EBS | IAM (gerenciamento) + SG/NACL (tráfego instância) | 
| EFS | SG + POSIX perms + Access Points | 
| FSx Windows | AD + NTFS ACL + SG | 
| FSx Lustre | POSIX perms + SG | 
| FSx ONTAP | Export policies + AD/LDAP + SG | 
| FSx OpenZFS | POSIX + SG |

## Observabilidade
- EBS: métricas Volume* no CloudWatch, CloudTrail para API snapshot.
- EFS: PercentIOLimit, BurstCreditBalance, DataReadIOBytes/WriteIOBytes.
- FSx: métricas específicas por flavor (Throughput, IOPS, MetadataOps).

---
## Resumo Final
EBS = blocos para performance e baixa latência por instância.
EFS = arquivo NFS elástico multi-AZ para compartilhamento simples.
FSx = arquivos especializados (Windows/SMB, HPC Lustre, enterprise ONTAP, low-latency OpenZFS). Selecionar baseado em protocolo, performance e gerenciamento esperado.
