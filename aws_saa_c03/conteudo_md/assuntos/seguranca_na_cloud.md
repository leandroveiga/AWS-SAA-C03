## Segurança na Nuvem AWS – Foco SAA-C03 2025

### Camadas Principais
| Camada | Serviço | Função |
|--------|---------|--------|
| Identidade | IAM / Identity Center / Organizations | Autenticação, autorização, guard rails |
| Detecção | CloudTrail / GuardDuty / Detective | Registro de ações, detecção ameaças, investigação |
| Proteção Dados | KMS / S3 Encryption / Secrets Manager | Criptografia e gestão de segredos |
| Proteção App | WAF / Shield / AWS Firewall Manager | Mitigação L7 / DDoS |
| Rede | Security Groups / NACL / Network Firewall | Controle tráfego |
| Conformidade | Config / Security Hub / Audit Manager / Artifact | Postura e evidências |

### Serviços-Chave
- **AWS KMS**: Chaves CMK gerenciadas / customer-managed; usar alias; habilitar key rotation. Envelope encryption (KMS cifra data keys que cifram dados reais).
- **Secrets Manager vs Parameter Store**: Secrets Manager para rotação automática credenciais (DB/Redshift/RDS), Parameter Store (SecureString) para config hierárquica (integra com AppConfig).
- **CloudTrail & CloudTrail Lake**: Lake para consultas avançadas (SQL) e retenção analítica de logs de auditoria.
- **GuardDuty**: Detecção (DNS anômalo, IAM credential exfiltration, Malware Protection EC2/EBS/EKS, RDS Protection). Integrar com Security Hub.
- **AWS Config**: Avaliação contínua de compliance (rules gerenciadas + custom Lambda). Grava histórico de configurações.
- **Security Hub**: Agrega achados (Findings) de múltiplos serviços (GuardDuty, Inspector, Macie, Config, etc.) e frameworks (CIS, PCI). 
- **Inspector**: Varredura de vulnerabilidades em EC2 (agent), ECR (image scanning), Lambda (package libs), e container images.
- **Macie**: Descoberta de PII em S3 com ML.
- **Detective**: Investigação de relacionamentos entre entidades (IAM principal, IP, recursos) usando gráficos.
- **Shield Standard / Advanced**: DDoS (L3/L4) proteção. Advanced adiciona tempos de resposta, proteção custo scaled, integração com WAF e suporte 24/7.
- **AWS WAF**: Regras L7 (SQLi, XSS, rate-based, managed rules). Aplicado em ALB / API Gateway / CloudFront / AppSync.
- **Network Firewall**: IDS/IPS stateful em nível VPC (Suricata). 
- **Access Analyzer**: Detecta recursos expostos inadvertidamente.
- **Artifact**: Relatórios de compliance (ISO, SOC) para auditorias.
- **AWS Backup**: Políticas centralizadas multi-serviço (EFS, RDS, DynamoDB, FSx, EC2 snapshots via tag, etc.).
- **Verified Access**: Zero Trust access a aplicações internas sem VPN tradicional.

### Criptografia – Padrões
| Camada | Opção | Observação |
|--------|-------|------------|
| S3 | SSE-S3 / SSE-KMS / Client-side | SSE-S3 default; SSE-KMS para audit key usage |
| EBS | Encryption at rest (KMS) | Pode recriptografar snapshots |
| RDS/Aurora | Transparent encryption | Ativar em criação (não reverte) |
| DynamoDB | Sempre ativado | KMS opcional custom key |
| TLS | ACM (renovação automática) | Usar policies modernas |

### Estratégias de Segregação
- Multi-conta (Organizations) + SCPs (negar *ec2:RunInstances* fora regiões permitidas, etc.).
- Tag-based ABAC (Attribute-Based Access Control) para escala de permissões.

### Padrões de Pergunta
1. Detectar credenciais IAM possivelmente comprometidas → GuardDuty finding + Access Analyzer review.
2. Garantir rotação secreta banco de dados sem downtime → Secrets Manager rotation Lambda.
3. Consolidar postura e conformidade multi-região → Security Hub + Config.
4. Inspecionar tráfego leste-oeste + bloquear malware → Network Firewall + GuardDuty Malware Protection.
5. Investigar incidente pivot entre recursos → Detective.

### Checklist Rápida
- CloudTrail multi-região + log integrity validation.
- KMS CMKs customer-managed para dados sensíveis (PII).
- Enforce MFA + evitar usuários IAM para workforce (usar Identity Center).
- Config + Security Hub integrados.
- S3 Block Public Access habilitado conta.
