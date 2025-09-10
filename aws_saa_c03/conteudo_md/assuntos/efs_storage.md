# Amazon EFS (Elastic File System) – Atualizado 2025

Escopo: sistema de arquivos NFS (Linux) totalmente gerenciado, elástico (cresce/encolhe automaticamente conforme gravação/remoção). Recomendado para workloads que precisam de POSIX multi-instância (conteúdo web, home directories, microserviços compartilhando código, analytics leve, backups de aplicações).

## Características Principais
- Protocolos: NFSv4 / v4.1
- Elástico: sem provisioning de tamanho; paga somente pelos GiB-mês efetivamente armazenados (por classe) + transferências.
- Multi-AZ: (Standard) dados replicados automaticamente em múltiplas AZs da região.
- One Zone: menor custo, armazenado em uma única AZ (menos alta disponibilidade).
- Performance Modes: General Purpose (latência baixa – maioria), Max I/O (maior throughput agregado, latência mais alta, clusters grandes de análise)
- Throughput Modes:
  - Bursting (padrão): créditos baseados no tamanho (baseline 50 KiB/s/GB + bursts).
  - Provisioned: define taxa (MiB/s) independente do tamanho (custo adicional).
  - Elastic (novo): ajusta automaticamente throughput (ideal variação imprevisível, custo baseado em uso real de throughput).
- Storage Classes / Tiers:
  - Standard / Standard-IA
  - One Zone / One Zone-IA
  - Lifecycle Management pode migrar arquivos inativos para IA (padrões: 7, 14, 30, 60, 90 dias).
- EFS Intelligent-Tiering (com Lifecycle + IA) reduz custo automaticamente para arquivos ociosos.
- EFS Access Points: abstração de acesso com root directory e UID/GID overrides (simplifica multi-tenant / aplicações containerizadas).
- Criptografia: at-rest (KMS) e in-transit (TLS) suportadas.
- Backup: integração com AWS Backup para políticas centralizadas.
- Replicação: EFS Replication (assíncrona cross-region ou in-region para DR) – RPO em minutos.

## Custos (Faixa Relativa)
| Item | Custo Relativo |
|------|----------------|
| EFS Standard | $$$ |
| EFS Standard-IA | $$ |
| EFS One Zone | $$ |
| EFS One Zone-IA | $ |
| Provisioned Throughput extra | +$ a +$$ |
| Elastic Throughput | Paga pelo uso real (pode reduzir picos) |

## Quando Usar
| Cenário | Motivo |
|---------|--------|
| Várias instâncias EC2 servindo site estático/dinâmico | Consistência de arquivos compartilhados |
| Aplicações containerizadas (ECS/EKS) com config/código compartilhado | Mount NFS simples |
| Home directories / perfis usuários | POSIX completo + permissões Unix |
| Ferramentas de build CI/CD compartilhadas | Cache de dependências compartilhado |

## Quando NÃO Usar
| Situação | Alternativa |
|----------|-------------|
| Latência single-digit microsegundos / IOPS altíssimo | EBS io2 / local NVMe |
| Arquivos raramente montados / long-term archive | S3 (Glacier) |
| Windows (SMB) compartilhado | FSx for Windows |
| Alto throughput HPC paralelo (Lustre) | FSx for Lustre |

## Limites / Capacidades
- Tamanho: virtualmente ilimitado (cresce sob demanda).
- Arquivo individual: até o limite de POSIX (prático > TB). 
- Throughput máximo: depende do modo (Bursting baseado em tamanho, Provisioned fixo até GiB/s, Elastic adaptativo).
- Montagens simultâneas: centenas por sistema (limitadas por limites regionais e sincronia NFS).

## Exemplo de Dimensionamento
- Aplicação com 500 GB em EFS Standard Bursting:
  - Baseline throughput ≈ 500 GB * 50 KiB/s = 25.000 KiB/s ≈ 24,4 MiB/s.
  - Bursts permitem picos muito superiores usando créditos acumulados.
  - Se throughput sustentado necessário for 80 MiB/s, considerar Provisioned ou Elastic Throughput.

## Lifecycle & Classes
| Classe | Replicação AZ | Casos de Uso | Observações |
|--------|---------------|-------------|-------------|
| Standard | Multi-AZ | Prod geral | Alta disponibilidade |
| Standard-IA | Multi-AZ | Arquivos frios ainda resilientes | Transição automática via lifecycle |
| One Zone | 1 AZ | Dev / staging / reprocessável | Menor custo, menor HA |
| One Zone-IA | 1 AZ | Dados frios não críticos | Combinar com backup |

## Access Points – Exemplo
Microserviços diferentes em ECS montam o mesmo EFS, cada um via Access Point:
- /appA (UID 1000)
- /appB (UID 1001)
Permite isolamento lógico sem criar múltiplos sistemas.

## Segurança
- Controlar acesso via SG + NACL (tráfego NFS TCP 2049). 
- IAM policy controla criação/gerenciamento, não o acesso POSIX.
- Use Access Points para evitar configurações erradas de permissões em containers.

## Boas Práticas
- Ajustar mode para General Purpose a menos que cluster massivo de analytics → Max I/O.
- Habilitar lifecycle para mover dados inativos p/ *-IA*.
- Medir utilização (PercentIOLimit). Se saturado continuamente → considerar Provisioned/Elastic Throughput.
- Usar EFS One Zone somente quando dados reconstruíveis / não críticos.

## Perguntas de Exame (Exemplos)
1. Necessário sistema de arquivos compartilhado multi-AZ para múltiplas instâncias Linux → EFS Standard.
2. Reduzir custo de arquivos ociosos sem mover manualmente → Lifecycle para IA.
3. Throughput sustentado imprevisível sem prever picos → Elastic Throughput.
4. Laboratório de desenvolvimento efêmero custo otimizado → EFS One Zone.
5. Aplicação container ECS com múltiplos tenants isolados por diretório → EFS Access Points.

---
## Resumo
EFS = file system compartilhado, elástico, multi-AZ (opcional), com classes IA e One Zone para otimização de custo. Ideal quando POSIX e múltiplos clientes simultâneos são requisitos.
