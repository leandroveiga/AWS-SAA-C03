## Arquiteturas de Aplicações na AWS – SAA-C03 2025

### Padrões Fundamentais
| Padrão | Objetivo | Serviços-Chave |
|--------|----------|----------------|
| Microserviços | Desacoplamento de domínios | ECS / EKS / Lambda / API Gateway |
| Event-Driven | Reatividade / Escalabilidade | EventBridge / SNS / SQS / Lambda / Kinesis |
| CQRS & Fan-out | Separar leitura/escrita / replicar eventos | DynamoDB Streams / Lambda / SNS |
| Caching | Reduzir latência / custo | ElastiCache (Redis/Memcached), CloudFront |
| Orquestração vs Choreography | Fluxo controlado vs desacoplado | Step Functions / EventBridge |
| Infra as Code | Consistência / repetibilidade | CloudFormation / CDK / SAM / Terraform |

### API & Integração
| Necessidade | Serviço | Observação |
|-------------|---------|------------|
| APIs HTTP simples | API Gateway HTTP API | Menor custo/latência |
| APIs REST completas | API Gateway REST API | Recursos, usage plans, modelos |
| WebSockets | API Gateway WebSocket | Estado conexão gerenciado |
| GraphQL | AppSync | Subscriptions em tempo real |
| Mensageria assíncrona | SQS | Buffer / retry / DLQ |
| Fan-out múltiplos consumers | SNS | Pub/Sub simples |
| Event bus empresarial | EventBridge | Filtros avançados, SaaS integration |
| Streaming ordenado | Kinesis Data Streams | Analytics near real-time |

### Estratégias de Resiliência
- Bulkhead (isolar componentes – ex.: filas por domínio).
- Circuit Breaker (gerenciar falhas remotas – libs ou Step Functions com retries controlados).
- Retry com backoff + jitter (evitar thundering herd).
- Dead Letter Queues (SQS/SNS/Lambda) para análise e replay.

### DR e Alta Disponibilidade (Resumo)
Ver também conceitos em `conceitos_cloud_computing.md`.

### Observabilidade & Telemetria
- CloudWatch (metrics/logs/alarms) + X-Ray tracing.
- Centralizar logs estruturados em JSON (facilita Insights).
- Distributed tracing obrigatório em microserviços (trace-id propagate via headers). 

### Step Functions vs Lambda Chain Manual
- Step Functions: visibilidade, retries configuráveis, parallel/map states, custo por state transition.
- Encadeamento manual: simples mas difícil de observar/manter.

### Caching Padrões
| Padrão | Descrição | Exemplo |
|--------|-----------|---------|
| Edge Caching | CDN para conteúdo estático/dinâmico cacheável | CloudFront + S3/ALB |
| Application Cache | Itens quentes / sessões | ElastiCache Redis |
| Database Query Cache | Reduzir repetição de queries | DAX para DynamoDB |

### Custos – Otimização Arquitetural
- Usar Spot para trabalhadores batch ou containers não críticos.
- Offload conteúdo estático para S3+CloudFront.
- Usar Lambda + EventBridge para orquestração leve (reduz custo de infra fixa).

### Segurança Embutida (Shift-Left)
- Templates CloudFormation com policy lint (cfn-nag).
- Scan de imagens container (ECR + Inspector). 
- GitOps (pull model) reduz drift.

### Padrões de Perguntas
1. Necessário integrar múltiplos SaaS emitindo eventos → EventBridge (SaaS partner event bus).
2. Processar 2M de pedidos streaming e consultas analytics near real-time → Kinesis Data Streams + Lambda / Analytics.
3. Diminuir acoplamento entre serviço de pagamento e envio email → SNS (fan-out) ou EventBridge se regras complexas.
4. Orquestrar workflow com branches, compensações → Step Functions (saga pattern).
5. Minimizar latência global de API não cacheável → Global Accelerator.

### Roadmap Estudo (Sugestão)
1. Fundamentos IAM, VPC, EC2, S3.
2. Padrões de desacoplamento (SQS/SNS/EventBridge/Kinesis).
3. Persistência (RDS/Aurora/DynamoDB/Redshift). 
4. Observabilidade e segurança (CloudWatch, X-Ray, GuardDuty, Config, KMS).
5. Otimização de custo e DR.
