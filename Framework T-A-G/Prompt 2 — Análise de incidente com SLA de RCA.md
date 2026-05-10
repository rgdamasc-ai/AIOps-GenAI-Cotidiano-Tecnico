# Experimento 02 — Análise de incidente com SLA de RCA

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

## Contexto

RCA preliminar de um incidente de timeout (HTTP 504) em API de pagamentos rodando em ECS Fargate + ALB + RDS PostgreSQL, com 30% das requisições afetadas. O Goal de "até 2 horas, priorizando menor risco" orientou a IA a priorizar ações observacionais antes de ações corretivas.

## Prompt usado

```markdown
# Task
Analise o seguinte cenário de incidente: o serviço de API de pagamentos está retornando timeout (HTTP 504) para 30% das requisições desde as 14h. O serviço roda em ECS Fargate com 4 tasks, ALB na frente e banco RDS PostgreSQL.

# Action
Faça uma análise estruturada: liste as 5 causas mais prováveis em ordem de probabilidade, para cada causa indique o comando ou métrica AWS para validar, e proponha a ação corretiva imediata.

# Goal
Objetivo: produzir uma análise de causa raiz preliminar em formato de documento que permita ao time de plantão identificar e corrigir a causa raiz em até 2 horas, priorizando ações de menor risco primeiro.
```

## Resposta da IA

A IA retornou um documento de RCA completo com 5 causas priorizadas por probabilidade, cada uma com comandos AWS CLI e queries PostgreSQL prontos para validação, ação corretiva graduada por risco, e uma tabela de resumo executivo com sequência de ação, risco e tempo estimado por etapa. O critério de resolução foi definido explicitamente ao final.

## Output completo da IA

# Análise de Causa Raiz Preliminar — Incidente HTTP 504

**Serviço:** API de Pagamentos
**Data/Hora de início:** 2026-05-10, ~14h00
**Sintoma:** 30% das requisições retornando HTTP 504 Gateway Timeout
**Stack:** ECS Fargate (4 tasks) + ALB + RDS PostgreSQL
**SLO de resolução:** até 2 horas

---

## Causa 1 — Esgotamento de conexões / alta latência no RDS PostgreSQL

**Probabilidade: Alta.** API de pagamentos tipicamente faz I/O intensivo em banco; pool de conexões é o gargalo mais frequente em 504s com esse stack.

### Validação

```bash
# Conexões ativas e estado no RDS (via CloudWatch CLI)
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name DatabaseConnections \
  --dimensions Name=DBInstanceIdentifier,Value=<sua-instancia-rds> \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T16:00:00Z \
  --period 300 \
  --statistics Average Maximum

# Latência de leitura/escrita
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name WriteLatency \
  --dimensions Name=DBInstanceIdentifier,Value=<sua-instancia-rds> \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T16:00:00Z \
  --period 60 \
  --statistics Average Maximum
```

```sql
-- Sessões ativas e em espera (snapshot imediato)
SELECT pid, state, wait_event_type, wait_event, query_start,
       now() - query_start AS duration, left(query, 120) AS query_snippet
FROM pg_stat_activity
WHERE state <> 'idle'
ORDER BY duration DESC
LIMIT 30;

-- Locks bloqueando outras queries
SELECT bl.pid AS blocked_pid, a.query AS blocked_query,
       kl.pid AS blocking_pid, ka.query AS blocking_query
FROM pg_catalog.pg_locks bl
JOIN pg_catalog.pg_stat_activity a ON a.pid = bl.pid
JOIN pg_catalog.pg_locks kl ON kl.transactionid = bl.transactionid AND kl.pid <> bl.pid
JOIN pg_catalog.pg_stat_activity ka ON ka.pid = kl.pid
WHERE NOT bl.granted;
```

### Ação corretiva imediata (risco baixo → alto)

1. Abrir RDS Performance Insights → identificar Top SQL por tempo de espera.
2. Matar queries com duração > 30s (sem impacto em dados confirmados):
   ```sql
   SELECT pg_terminate_backend(pid)
   FROM pg_stat_activity
   WHERE state = 'active' AND now() - query_start > interval '30 seconds'
     AND query NOT ILIKE '%pg_terminate%';
   ```
3. Aumentar `max_connections` via parameter group (requer reboot — só se confirmado como causa).
4. Avaliar adicionar **RDS Proxy** para pooling gerenciado (ação de médio prazo).

---

## Causa 2 — Recursos das ECS Tasks esgotados (CPU ou memória)

**Probabilidade: Alta.** Com 4 tasks fixas e spike de tráfego, cada task pode estar com CPU throttled ou swap de memória, causando respostas lentas e timeout no ALB.

### Validação

```bash
aws cloudwatch get-metric-statistics \
  --namespace AWS/ECS \
  --metric-name CPUUtilization \
  --dimensions Name=ClusterName,Value=<cluster> Name=ServiceName,Value=<servico-pagamentos> \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T16:00:00Z \
  --period 60 \
  --statistics Average Maximum

# Tasks em execução vs desejadas
aws ecs describe-services \
  --cluster <cluster> \
  --services <servico-pagamentos> \
  --query 'services[0].{desired:desiredCount,running:runningCount,pending:pendingCount}'
```

### Ação corretiva imediata

1. **Scale out manual imediato** (baixo risco, reversível):
   ```bash
   aws ecs update-service \
     --cluster <cluster> \
     --service <servico-pagamentos> \
     --desired-count 8
   ```
2. Verificar e ativar Application Auto Scaling no serviço ECS para prevenir recorrência.

---

## Causa 3 — Timeout do ALB mal configurado ou targets com falha de health check

**Probabilidade: Média.** Se o timeout idle do ALB (padrão: 60s) for menor que o tempo de resposta do backend, o ALB encerra a conexão e retorna 504.

### Validação

```bash
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApplicationELB \
  --metric-name TargetResponseTime \
  --dimensions Name=LoadBalancer,Value=<arn-do-alb-sufixo> \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T16:00:00Z \
  --period 60 \
  --statistics Average p99

# Targets saudáveis no target group
aws elbv2 describe-target-health \
  --target-group-arn <arn-do-target-group>
```

### Ação corretiva imediata

1. Verificar saúde dos targets — se < 4 saudáveis, investigar causa das falhas.
2. **Aumentar idle timeout do ALB** (sem downtime, efeito imediato):
   ```bash
   aws elbv2 modify-load-balancer-attributes \
     --load-balancer-arn <arn-do-alb> \
     --attributes Key=idle_timeout.timeout_seconds,Value=120
   ```

---

## Causa 4 — Query lenta ou lock de longa duração (introduzida por deploy)

**Probabilidade: Média.** Se houve deploy próximo das 14h, uma query com full-table scan ou transação longa pode estar bloqueando operações.

### Validação

```bash
aws ecs describe-services \
  --cluster <cluster> \
  --services <servico-pagamentos> \
  --query 'services[0].events[:10]'
```

```sql
SELECT query, calls, mean_exec_time, total_exec_time
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;
```

### Ação corretiva imediata

1. **Rollback do deploy** se houver correlação temporal clara.
2. **Criar índice CONCURRENTLY** na coluna identificada (sem lock na tabela):
   ```sql
   CREATE INDEX CONCURRENTLY idx_payments_created_at ON payments(created_at);
   ```

---

## Causa 5 — Problema de rede: Security Group, VPC routing ou NAT Gateway

**Probabilidade: Baixa-média.** Mudança acidental em Security Group ou saturação do NAT Gateway pode bloquear conectividade entre ECS tasks e RDS.

### Validação

```bash
aws cloudtrail lookup-events \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T14:30:00Z \
  --lookup-attributes AttributeKey=EventName,AttributeValue=RevokeSecurityGroupEgress

aws cloudwatch get-metric-statistics \
  --namespace AWS/NatGateway \
  --metric-name ErrorPortAllocation \
  --dimensions Name=NatGatewayId,Value=<nat-gw-id> \
  --start-time 2026-05-10T13:00:00Z \
  --end-time 2026-05-10T16:00:00Z \
  --period 60 \
  --statistics Sum
```

### Ação corretiva imediata

1. Reverter a mudança no Security Group identificada no CloudTrail.
2. Ativar VPC Reachability Analyzer entre task e RDS para diagnóstico preciso.

---

## Resumo executivo — Sequência de ação recomendada

| Prioridade | Ação | Risco | Tempo estimado |
|---|---|---|---|
| 1 | Abrir RDS Performance Insights → Top SQL | Zero | 2 min |
| 2 | Verificar `pg_stat_activity` + locks ativos | Zero | 5 min |
| 3 | Checar `HealthyHostCount` no target group do ALB | Zero | 2 min |
| 4 | Verificar CPU/Memória das ECS tasks no CloudWatch | Zero | 3 min |
| 5 | Scale out para 8 tasks ECS se CPU/Mem > 80% | Baixo | 5 min |
| 6 | Matar queries bloqueantes com `pg_terminate_backend` | Baixo | 5 min |
| 7 | Aumentar idle timeout do ALB se p99 > timeout | Baixo | 2 min |
| 8 | Verificar CloudTrail por deploys e mudanças de SG nas últimas 2h | Zero | 10 min |
| 9 | Rollback do deploy se correlação temporal confirmada | Médio | 15 min |

**Critério de resolução:** `HTTPCode_ELB_504_Count` retornando a zero por 5 minutos consecutivos com taxa de erro < 1%.

## O que funcionou

- O Goal com SLO de 2 horas e critério "menor risco primeiro" fez a IA ordenar a tabela final por risco (Zero → Baixo → Médio) em vez de por probabilidade — exatamente o que o plantão precisa
- Comandos AWS CLI e queries PostgreSQL foram gerados prontos para execução, com placeholders explícitos
- A IA incluiu critério de resolução mensurável (`HTTPCode_ELB_504_Count` = 0 por 5 min) sem ser solicitada
- O Cause 4 (deploy como gatilho) foi incluído com lógica de rollback — cobertura além do óbvio técnico

## O que não funcionou

- Os placeholders `<cluster>`, `<sua-instancia-rds>`, `<arn-do-alb>` exigem substituição manual — não há como a IA inferir valores reais sem contexto adicional
- Causa 5 (rede/SG) ficou com nível de detalhe menor que as causas 1 e 2 — provavelmente pela menor probabilidade, mas poderia ser expandida se o cenário sugerir mudanças recentes de infra

## Ajustes que eu fiz

- Nenhum ajuste necessário na primeira iteração. Para uso real no incidente, acrescentaria ao prompt os nomes reais do cluster, instância RDS e ARN do ALB para eliminar os placeholders e gerar comandos executáveis direto

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O Goal com SLO e critério de risco transformou uma lista genérica de causas em um runbook executável com sequência de ação priorizada. Output utilizável diretamente pelo plantão sem reformulação.

## O que eu faria diferente

Em um incidente real, adicionaria ao Task o timestamp exato do início, se houve deploy recente e a carga atual de TPS — esses dados fariam a IA elevar ou rebaixar probabilidades de forma mais precisa e possivelmente eliminar causas irrelevantes antes de listá-las.
