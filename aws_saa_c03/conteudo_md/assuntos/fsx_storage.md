# Amazon FSx – Família de Sistemas de Arquivos (2025)

Amazon FSx fornece sistemas de arquivos otimizados e compatíveis com padrões de mercado para workloads específicas. Cada flavor oferece performance, protocolo e semântica distintos.

## Variantes Principais
| Serviço | Protocolos / Acesso | Casos de Uso | Observações |
|---------|---------------------|-------------|-------------|
| FSx for Windows File Server | SMB (CIFS), AD integration | Aplicações Windows, homedir corporativo, lift-and-shift | Suporte DFS, Shadow Copies, ACL NTFS, Single-AZ ou Multi-AZ |
| FSx for Lustre | POSIX (alta performance) | HPC, ML, media rendering, analytics | Integração direta com S3 (import/export), throughput e IOPS altos, escalável por cluster |
| FSx for NetApp ONTAP | NFS, SMB, iSCSI | Workloads híbridos enterprise, migração Data Center | Snapshots, clones, thin provisioning, multi-protocolo |
| FSx for OpenZFS | NFS | Build farms, dev environments, baixa latência single-digit ms | Snapshots rápidos, clones copy-on-write |
| FSx for ONTAP (SnapLock) | NFS, SMB | Compliance WORM | Bloqueio de retenção regulatória |

## Comparativo de Características
| Critério | Windows | Lustre | ONTAP | OpenZFS |
|---------|---------|--------|-------|---------|
| Alta Performance HPC | ✖ | ✔✔ | ✔ (bom) | ✔ (moderado) |
| Integr. Active Directory | ✔ | ✖ | ✔ (via AD) | ✖ |
| Multi-Protocolo (NFS + SMB) | ✖ | ✖ | ✔ | ✖ |
| Integração S3 Nativa | Limitada (copiar) | ✔ (data repository) | Via ferramentas | Não nativa |
| Snapshots & Clones Avançados | Básico (Shadow Copy) | Limitado | ✔ (FlexClone etc.) | ✔ (ZFS snapshots) |
| Custo Geral Relativo | $$$ | $$–$$$$ (conforme perf) | $$$ | $$ |
| Latência Micro-baixa | ✖ | Alta taxa throughput | Boa | Boa |

## Modelos de Deploy / HA
| Serviço | HA | Detalhes |
|---------|----|---------|
| Windows | Single-AZ ou Multi-AZ (failover automático) | Multi-AZ recomendado para produção |
| Lustre | Single-AZ (principalmente) | Pode recriar de S3 metadata; foco performance |
| ONTAP | Multi-AZ (ha pair) | Alta resiliência + replicação snapmirror |
| OpenZFS | Single-AZ (atual) | Backup/replicação snapshots para DR |

## Performance (Visão Geral)
- Lustre: terabytes por segundo em larga escala (através de múltiplos file servers). Conectar a S3 data repository para burst.
- Windows: throughput e IOPS ajustados pelo tamanho de file system e capacidade provisionada.
- ONTAP: escalabilidade vertical + eficiência (dedupe/compression). 
- OpenZFS: baixa latência para workloads de compilação e CI.

## Custos Relativos (Muito Aproximado)
| Serviço | Faixa Custo |
|---------|-------------|
| FSx Windows | $$$ |
| FSx Lustre | $$ (standard) até $$$$ (alta performance) |
| FSx ONTAP | $$$ |
| FSx OpenZFS | $$ |

## Casos de Uso Resumidos
| Caso | Escolha | Justificativa |
|------|---------|---------------|
| Levantar aplicação .NET legado lift-and-shift | Windows | Suporte SMB + ACL NTFS |
| Treino de modelo ML com dataset em S3 | Lustre | Throughput massivo + export/import S3 |
| Ambiente híbrido com storage enterprise existente NetApp | ONTAP | Snapshot, replicação SnapMirror, multi-protocolo |
| Farm de builds (C/C++/Android) | OpenZFS | Latência baixa + snapshots rápidos |
| Requisito WORM compliance | ONTAP (SnapLock) | Trilha imutável |

## Segurança e Integrações
- Windows: integra-se a Active Directory, permissões NTFS, criptografia KMS.
- Lustre: POSIX perms tradicionais; criptografia em repouso; controle via SG.
- ONTAP: export policies, AD/LDAP, snapshots, tiering para S3 (FabricPool quando suportado).
- OpenZFS: NFS ACL + snapshots; backup via AWS Backup.

## Perguntas de Exame (Exemplos)
1. App Windows precisa compartilhamento SMB multi-AZ → FSx Windows Multi-AZ.
2. HPC precisa processar dataset de petabytes no S3 rapidamente → FSx Lustre (import data repository, export depois).
3. Empresa possui NetApp on-prem e quer replicação para nuvem → FSx ONTAP (SnapMirror).
4. Build pipeline precisa snapshots clonáveis rápido → FSx OpenZFS.
5. Requisito WORM regulatório em filesystem → FSx ONTAP SnapLock.

## Boas Práticas
- Dimensionar performance Lustre por throughput/metadata vs preço.
- Windows: usar Multi-AZ para workloads sempre-on críticos.
- ONTAP: aplicar tiers automáticos para reduzir custo (dados frios → capacity tier).
- Monitorar métricas (CloudWatch) de throughput, I/O, latência e metadata ops.

---
## Resumo
FSx = família especializada. Escolha baseada em protocolo, performance e compatibilidade. Use EFS para NFS simples elástico; FSx quando precisa recursos avançados (SMB, HPC extremo, multi-protocolo, snapshots enterprise).
