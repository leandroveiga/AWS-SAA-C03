## Introdução ao Elastic Load Balancer (ELB)

O Elastic Load Balancer (ELB) é um serviço da Amazon Web Services (AWS) projetado para distribuir o tráfego de rede de maneira uniforme entre várias instâncias de Amazon Elastic Compute Cloud (EC2) ou recursos de backend, melhorando a escalabilidade e a disponibilidade das aplicações.

**Exemplo:** Se você possui várias instâncias EC2 executando uma aplicação web, o ELB pode distribuir o tráfego entre elas, ajudando a evitar sobrecargas em uma única instância.

## Diferença entre Scaling UP x Scaling Out

- **Scaling UP:** Refere-se ao aumento da capacidade de uma única instância ou recurso. Isso é feito aumentando os recursos, como CPU e RAM, de uma única máquina.

- **Scaling Out:** Envolve adicionar mais instâncias ou recursos semelhantes. É uma abordagem horizontal, onde novas instâncias são adicionadas para lidar com um maior volume de tráfego.

**Exemplo:** Se sua aplicação web estiver enfrentando um aumento de tráfego, você pode escalar horizontalmente (Scaling Out) adicionando mais instâncias EC2 ao ELB.

## Conhecendo o EC2 Auto Scaling

O Amazon EC2 Auto Scaling é um serviço que ajuda a manter a escalabilidade automática das instâncias EC2, ajustando o número de instâncias com base nas métricas configuradas.

**Exemplo:** Você pode configurar o Auto Scaling para adicionar mais instâncias EC2 ao seu grupo quando a CPU média das instâncias existentes atingir um limite predefinido.

## Introdução ao Load Balancer

Um Load Balancer é um dispositivo ou serviço que distribui o tráfego de rede entre múltiplos destinos, garantindo que as solicitações dos clientes sejam encaminhadas de maneira eficaz para os recursos de backend.

**Exemplo:** Quando um usuário acessa um site, o Load Balancer decide para qual servidor a solicitação deve ser direcionada com base em métricas de saúde e algoritmos de balanceamento.

## Tipos de Load Balancers (Atual 2025)

| Tipo | Camada | Casos de Uso | Extras |
|------|--------|--------------|--------|
| Application Load Balancer (ALB) | L7 (HTTP/HTTPS/WebSocket/HTTP2/gRPC) | Apps web, microserviços, path/host routing | Regras avançadas, header/query, WAF, auth OIDC, fixed/weighted target routing |
| Network Load Balancer (NLB) | L4 (TCP/UDP/TLS) | Alta performance, baixa latência, milhões de conexões | Static IP / Elastic IP, TLS pass-through/termination, zonal health |
| Gateway Load Balancer (GWLB) | Encapsula tráfego (GENEVE) | Inserção transparente de appliances (firewalls, IDS) | Escala horizontal chain de inspeção |
| Classic (CLB) | (LEGADO) | Evitar em novas arquiteturas | Migrar para ALB/NLB |

### Cross-Zone Load Balancing
- ALB: Sempre ativado (sem cobrança extra).
- NLB: Opcional (custo de LCU adicional). Se desativado, preserva proporção por AZ.

### Target Groups
- Tipos: Instance, IP, Lambda, Application Load Balancer (chaining em cenários limitados).
- Health check por target group (HTTP codes custom, healthy threshold, path).

### Stickiness
- ALB cookie gerenciado (app_lb cookie) ou cookie baseado em aplicação.
- NLB: Source IP stickiness.

### Segurança
- WAF somente integrado nativamente ao ALB/CloudFront/APIGW (não ao NLB diretamente).
- TLS Offload em ALB: usar certs ACM + segurança (policies TLS) >= TLS1.2.

---
## Auto Scaling (EC2 Auto Scaling & Application Auto Scaling)

### Conceitos
- Group: coleção lógica de instâncias.
- Launch Template (preferido) vs Launch Configuration (legacy, evitar em novas implantações).
- Desired / Min / Max capacity.

### Políticas de Scaling
| Tipo | Uso | Descrição |
|------|-----|-----------|
| Target Tracking | Manter métrica alvo (ex. 50% CPU) | Auto-ajuste proporcional |
| Step Scaling | Respostas escalonadas a alarmes | Granular em picos previsíveis |
| Simple (Legacy) | Ação única por alarme | Evitar novas | 
| Scheduled | Cargas previsíveis (horário comercial) | Ajuste programado |
| Predictive | Machine learning (padrões históricos) | Cargas sazonais |

### Warm Pools
Mantêm instâncias pré-inicializadas reduzindo cold start em bursts.

### Instance Refresh
Permite rolling update automático (ex.: nova AMI) com controle de taxa e health checks.

### Scale-In Protection
Evita que instâncias específicas sejam encerradas durante eventos de scale-in (uso temporário: debugging/migração).

### Métricas e Otimização
- Usar métricas compostas (ALB RequestCountPerTarget) para dimensionar horizontalmente camadas HTTP.
- CPU média não reflete sempre saturação (latência p95 pode ser melhor alvo via CloudWatch metric math + target tracking custom via Application Auto Scaling + custom metric).

### Perguntas Típicas
1. Rotear tráfego por path /images/ e /api/ → ALB com regras path.
2. Necessário endereço IP fixo + milhões de conexões TCP → NLB.
3. Inserir firewall de terceiros em fluxo de tráfego escalável → Gateway Load Balancer.
4. Diminuir cold start ao aumentar repentino de tráfego → Warm Pool.
5. Evitar dependência de Launch Configuration legado → Migrar para Launch Template.

---

## Auto Scaling com Application Load Balancer (ALB)

O Application Load Balancer (ALB) pode ser usado em conjunto com o Amazon EC2 Auto Scaling para dimensionar automaticamente os recursos conforme necessário. O ALB distribuirá o tráfego entre as instâncias escaladas automaticamente.

**Exemplo:** Se a carga aumentar em seu aplicativo, o Auto Scaling pode adicionar novas instâncias EC2 e o ALB irá distribuir o tráfego entre elas.

Esses tópicos abrangem os fundamentos dos Load Balancers e como eles são usados em conjunto com o Auto Scaling na AWS. Se você tiver dúvidas adicionais ou precisar de exemplos mais específicos, sinta-se à vontade para perguntar.

# Route53

O **Route53** é um serviço de DNS (Domain Name System) oferecido pela AWS (Amazon Web Services). Ele permite que você registre e gerencie nomes de domínio, como exemplo.com, e associe esses nomes a recursos da AWS, como instâncias EC2, balanceadores de carga, buckets do S3, entre outros.

### Políticas de Roteamento (Exame)
- Simple
- Weighted
- Latency-Based
- Failover (Primary / Secondary com health checks)
- Geolocation
- Geoproximity (Traffic Flow) – pode usar bias
- Multi-Value Answer (até 8 registros healthy – pseudo load balance)
- IP-based (endereçar ranges específicos)

### Health Checks
- Podem monitorar endpoint HTTP/HTTPS/TCP, integrar CloudWatch Alarms e influenciar failover.

### Private Hosted Zones
- Resolução interna para VPCs. Associar múltiplas VPCs (mesma ou diferentes contas via RAM + autorização).

### Resolver Endpoints
- Inbound: permitir on-prem → resolver DNS privado de VPC.
- Outbound: resolver nomes on-prem a partir da VPC (rules condicionais).
- DNS Firewall: bloquear domínios maliciosos (lista gerenciada + custom). 

### Perguntas Típicas
1. Direcionar usuários à região mais próxima → Latency policy.
2. Controlar gradualmente rollout 10/90 → Weighted policy.
3. DR ativo-passivo → Failover + health check.
4. Bloquear exfiltração DNS → DNS Firewall.

## Principais recursos do Route53

- **Registro de domínio**: o Route53 permite que você registre novos domínios diretamente através do serviço. Ele também oferece a opção de transferir domínios existentes de outros registradores para a AWS.

- **Gerenciamento de DNS**: o Route53 permite que você configure e gerencie registros DNS para seus domínios. Isso inclui a criação de registros A, CNAME, MX, TXT, entre outros.

- **Resolução de DNS**: o Route53 é responsável por resolver solicitações de DNS e direcioná-las para os recursos corretos da AWS. Ele oferece alta disponibilidade e baixa latência na resolução de DNS.

- **Roteamento de tráfego**: o Route53 permite que você configure regras de roteamento de tráfego com base em políticas de balanceamento de carga, geolocalização, latência, entre outros. Isso permite que você distribua o tráfego entre diferentes recursos da AWS de forma eficiente.

## Exemplos de uso do Route53

- **Registro de domínio**: você pode usar o Route53 para registrar um novo domínio, como exemplo.com, e associá-lo aos recursos da AWS que desejar.

- **Configuração de registros DNS**: você pode usar o Route53 para configurar registros DNS, como registros A para direcionar um domínio para um endereço IP específico, registros CNAME para criar aliases de domínio, registros MX para configurar servidores de e-mail, entre outros.

- **Balanceamento de carga**: o Route53 pode ser usado para configurar políticas de balanceamento de carga, distribuindo o tráfego entre diferentes instâncias EC2 ou outros recursos da AWS.

- **Failover**: o Route53 permite configurar políticas de failover, redirecionando o tráfego para recursos de backup em caso de falha dos recursos primários.

- **Geolocalização**: o Route53 permite direcionar o tráfego com base na localização geográfica dos usuários, redirecionando-os para servidores mais próximos.

Esses são apenas alguns exemplos de como o Route53 pode ser utilizado. Ele oferece uma ampla gama de recursos para gerenciar e direcionar o tráfego de DNS em sua infraestrutura na AWS.