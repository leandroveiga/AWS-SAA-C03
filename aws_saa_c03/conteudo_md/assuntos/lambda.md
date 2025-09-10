## AWS Lambda (Serverless Compute) – Atualizado SAA-C03 2025

### Características Principais
- Compute gerenciado orientado a eventos.
- Cobrança por duração x memória provisionada (ms) + invocações.
- Escala automática granular (instâncias de execução isoladas por função/versão/Alias).

### Modelos de Invocação
| Tipo | Exemplo Fonte | Comportamento |
|------|---------------|---------------|
| Síncrono | API Gateway (REST/HTTP), ALB, Cognito triggers | Cliente aguarda resposta |
| Assíncrono | S3 (Object Created), SNS, EventBridge | Retries automáticos + DLQ / Destinations |
| Stream/Poll-based | Kinesis, DynamoDB Streams, SQS (standard/FIFO) | Lambda faz poll; batch + checkpoint |

### Concurrency
- Concurrency padrão ilimitada por conta/região (sujeita a soft limit). 
- Reserved Concurrency: garante e limita simultâneo (isola função crítica).
- Provisioned Concurrency: pré-aquece ambientes reduzindo cold start (padrão para latência consistente).

### SnapStart (Java)
- Inicializa snapshot de execução após init, diminuindo cold start (apenas Java runtime compatível).

### Camadas e Extensões
- Layers para compartilhamento de libs.
- Extensions para observabilidade/segurança (ex.: Datadog, New Relic, custom logging).

### Integrações Frequentes
- API Gateway / ALB (front HTTP)
- EventBridge (event bus + regras avançadas)
- S3 (notificações)
- DynamoDB Streams / Kinesis (processamento streaming)
- SQS (buffer / decoupling)
- Step Functions (orquestração)

### Erros e Destinations
- Assíncrono: 2 tentativas adicionais (exponencial). Após falha → DLQ (SQS/SNS) ou Destination (Success/Failure) para outra função/EventBridge/SQS/SNS.
- Streams: retry até sucesso ou expiração janela + envio para *bisect batch* em alguns casos.

### Variáveis de Ambiente & Segredos
- Usar KMS para criptografar (chave gerenciada) / preferir Secrets Manager ou Parameter Store (SecureString) para credenciais.

### VPC
- Acesso a recursos privados: criar ENIs (pode aumentar cold start). Otimização recente reduz overhead quando reusando execução.
- Para acessar Internet mantendo em sub-rede privada: NAT Gateway + rota ou usar saída via endpoints privados (S3/DynamoDB) quando possível.

### Limites Importantes (podem mudar – sempre validar docs)
- Timeout máximo padrão: 15 min.
- Ephemeral storage /tmp: default 512 MB (pode aumentar até 10 GB).
- Memória: 128 MB – 10 GB (proporcional CPU / rede).

### Observabilidade
- CloudWatch Logs (automático), Metrics (Invocations, Duration, Errors, Throttles, IteratorAge), X-Ray para tracing, Lambda Telemetry API.

### Deployment & Versioning
- Publicar nova versão (imutável) → apontar Alias (ex.: prod) → Blue/Green / Canary (com CodeDeploy). 

### Boas Práticas de Código
- Idempotência (especial stream reprocessing).
- Reuso de conexões fora do handler para reduzir overhead init.
- Ajustar tamanho de batch stream vs latência.

### Perguntas Típicas
1. Latência consistente baixa para API crítica → Provisioned Concurrency.
2. Processar eventos S3 e aplicar transformação → Lambda + Event (Assíncrono) + DLQ.
3. Minimizar cold start Java → SnapStart.
4. Evitar saturação RDS por bursts → Lambda + RDS Proxy.
5. Executar tarefas sequenciais de compensação → Step Functions + Lambda.
