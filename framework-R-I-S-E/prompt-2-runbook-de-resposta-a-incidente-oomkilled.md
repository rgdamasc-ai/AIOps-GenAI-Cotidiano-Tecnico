# Prompt 2 — Runbook de resposta a incidente OOMKilled

**Framework:** R.I.S.E. (Role, Input, Steps, Expectation)

**Cenário:** Runbook operacional para incidente de OOMKilled em pods de pagamentos no EKS. Steps detalham a sequência de investigação (logs, métricas, container vs sidecar, mitigação) e Expectation define entregáveis (kubectl, PromQL, árvore de decisão, tempo máximo).

```markdown
# Role
Você é um SRE sênior com 8 anos de experiência em Kubernetes, observabilidade com Prometheus e Grafana, e resposta a incidentes de alta severidade (SEV1/SEV2) em ambientes de produção AWS.

# Input
- Ambiente: cluster EKS (Kubernetes 1.29) com 15 nodes em us-east-1
- Serviço afetado: API de pagamentos (namespace: payments)
- Sintoma: 3 pods em OOMKilled nos últimos 30 minutos
- Configuração atual: memory request 512Mi, memory limit 1Gi
- Prometheus mostra consumo de memória crescente desde o deploy v2.4.1 (há 2 horas)
- Nenhum outro namespace afetado
- SLA do serviço: 99.95% uptime, latência p99 < 500ms

# Steps
1. Confirmar o estado atual dos pods e coletar logs de OOMKilled
2. Analisar métricas de memória no Prometheus (últimas 4 horas, correlacionar com o deploy)
3. Verificar se o problema é no container principal ou no sidecar (istio-proxy/envoy)
4. Avaliar se é memory leak ou se o sizing está subdimensionado
5. Definir ação imediata de mitigação (ajuste de limits, rollback ou scale horizontal)
6. Documentar critérios de escalação (quando envolver o time de desenvolvimento)
7. Propor ajustes definitivos e critério de validação pós-fix

# Expectation
Espero um runbook operacional completo com:
- Comandos kubectl prontos para copiar e colar em cada etapa
- Queries PromQL para investigação de memória
- Árvore de decisão: leak vs subdimensionamento → ação correspondente
- Tempo máximo por etapa (total não deve passar de 45 minutos)
- Critérios claros de quando escalar para SEV1 e acionar o time de desenvolvimento
- Critério de rollback: quando reverter para v2.3.9 ao invés de tentar ajustar
```

---

# Experimento 02 — Runbook de incidente OOMKilled em pods de pagamentos (EKS)

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Troubleshooting

## Contexto

Incidente SEV2 em cluster EKS (Kubernetes 1.29): 3 pods da API de pagamentos (namespace:
payments) em OOMKilled nos últimos 30 minutos. Memory request 512Mi / limit 1Gi. Prometheus
indica consumo crescente desde deploy v2.4.1 (há 2 horas). SLA: 99,95% uptime, latência
p99 < 500ms.

## Prompt usado

```markdown
# Role
Você é um SRE sênior com 8 anos de experiência em Kubernetes, observabilidade com
Prometheus e Grafana, e resposta a incidentes de alta severidade (SEV1/SEV2) em
ambientes de produção AWS.

# Input
- Ambiente: cluster EKS (Kubernetes 1.29) com 15 nodes em us-east-1
- Serviço afetado: API de pagamentos (namespace: payments)
- Sintoma: 3 pods em OOMKilled nos últimos 30 minutos
- Configuração atual: memory request 512Mi, memory limit 1Gi
- Prometheus mostra consumo de memória crescente desde o deploy v2.4.1 (há 2 horas)
- Nenhum outro namespace afetado
- SLA do serviço: 99.95% uptime, latência p99 < 500ms

# Steps
1. Confirmar o estado atual dos pods e coletar logs de OOMKilled
2. Analisar métricas de memória no Prometheus (últimas 4 horas, correlacionar com o deploy)
3. Verificar se o problema é no container principal ou no sidecar (istio-proxy/envoy)
4. Avaliar se é memory leak ou se o sizing está subdimensionado
5. Definir ação imediata de mitigação (ajuste de limits, rollback ou scale horizontal)
6. Documentar critérios de escalação
7. Propor ajustes definitivos e critério de validação pós-fix

# Expectation
Espero um runbook operacional completo com:
- Comandos kubectl prontos para copiar e colar em cada etapa
- Queries PromQL para investigação de memória
- Árvore de decisão: leak vs subdimensionamento → ação correspondente
- Tempo máximo por etapa (total não deve passar de 45 minutos)
- Critérios claros de quando escalar para SEV1 e acionar o time de desenvolvimento
- Critério de rollback: quando reverter para v2.3.9 ao invés de tentar ajustar
```

## Resposta da IA

O modelo gerou um runbook operacional completo com 7 etapas, cada uma com tempo máximo
definido (total: 45 minutos), comandos kubectl copy-paste-ready, 4 queries PromQL para
investigação de memória, árvore de decisão binária (leak vs subdimensionamento), critérios
objetivos de escalação para SEV1, critérios de rollback e validação pós-fix. Cobriu todos
os entregáveis do Expectation em uma única execução.

## Output completo da IA

# Runbook de Incidente: OOMKilled — API de Pagamentos (namespace: payments)

**Severidade inicial:** SEV2 | **Janela de resolução:** 45 minutos

---

## ETAPA 1 — Confirmar estado atual dos pods e coletar logs de OOMKilled
**Tempo máximo: 5 minutos**

```bash
# Verificar estado dos pods no namespace payments
kubectl get pods -n payments -o wide

# Identificar pods em OOMKilled
kubectl describe pods -n payments | grep -A 10 "OOMKilled\|Last State"

# Coletar eventos recentes no namespace
kubectl get events -n payments --sort-by='.lastTimestamp' | tail -30

# Coletar logs do pod antes de morrer (previous container)
kubectl logs <pod-name> -n payments --previous -c payments-api | tail -200

# Confirmar OOMKilled via saída do pod
kubectl get pod <pod-name> -n payments \
  -o jsonpath='{.status.containerStatuses[*].lastState.terminated}'
```

**Critério de conclusão:** Confirmado que todos os 3 pods têm `reason: OOMKilled` no lastState.

---

## ETAPA 2 — Analisar métricas de memória no Prometheus
**Tempo máximo: 10 minutos**

**Queries PromQL — copie e cole no Grafana/Prometheus:**

```promql
# Consumo de memória dos containers de pagamentos (últimas 4h)
container_memory_working_set_bytes{namespace="payments", container="payments-api"}[4h]

# Taxa de crescimento de memória (MB/min) — detecta leak
rate(container_memory_working_set_bytes{
  namespace="payments", container="payments-api"}[10m]) * 60 / 1024 / 1024

# Memória em relação ao limit (% de uso)
container_memory_working_set_bytes{namespace="payments"}
  / container_spec_memory_limit_bytes{namespace="payments"} * 100

# OOMKills nas últimas 4h
kube_pod_container_status_last_terminated_reason{
  namespace="payments", reason="OOMKilled"}
```

**Correlação com deploy:** Se o gráfico mostra salto em memória no timestamp exato do
deploy v2.4.1, confirma relação causal.

---

## ETAPA 3 — Verificar container principal vs sidecar
**Tempo máximo: 7 minutos**

```bash
# Listar todos os containers dentro do pod
kubectl get pod <pod-name> -n payments \
  -o jsonpath='{.spec.containers[*].name}'

# Verificar consumo de memória por container
kubectl top pod <pod-name> -n payments --containers

# Verificar limits por container
kubectl describe pod <pod-name> -n payments | grep -A 5 "Limits\|Requests"
```

**Query PromQL por container:**

```promql
container_memory_working_set_bytes{namespace="payments", pod=~"payments-.*"}
  by (container, pod)
```

**Critério de conclusão:** Identificado qual container consome memória acima do esperado.

---

## ETAPA 4 — Avaliar: Memory Leak vs Subdimensionamento
**Tempo máximo: 5 minutos**

### Árvore de Decisão

```
Memória crescente de forma contínua e linear após o deploy?
├─ SIM → Suspeita de memory leak
│   └─ Taxa de crescimento > 50MB/hora?
│       ├─ SIM → MEMORY LEAK CONFIRMADO → Ir para Etapa 5A (rollback)
│       └─ NÃO → Aquecimento de cache → Observar 10min e retestar
└─ NÃO → Pico fixo após deploy, sem crescimento?
    └─ SIM → SUBDIMENSIONAMENTO
        └─ Novo pico > 1Gi (limit atual)?
            ├─ SIM → Ajustar memory limit → Ir para Etapa 5B
            └─ NÃO → Investigar spike pontual → logs da aplicação
```

---

## ETAPA 5 — Ação Imediata de Mitigação
**Tempo máximo: 10 minutos**

### 5A — ROLLBACK (memory leak confirmado)

**Critério de rollback para v2.3.9 — execute rollback se:**
- Taxa de crescimento de memória > 50MB/hora E crescimento linear confirmado
- Dois ou mais pods reiniciaram por OOMKilled após ajuste de limits
- Latência p99 ultrapassou 500ms (violação de SLA)

```bash
# Verificar histórico de revisões
kubectl rollout history deployment/payments-api -n payments

# Executar rollback para v2.3.9
kubectl rollout undo deployment/payments-api -n payments \
  --to-revision=<revision-v2.3.9>

# Acompanhar rollback
kubectl rollout status deployment/payments-api -n payments

# Confirmar versão após rollback
kubectl describe deployment payments-api -n payments | grep Image
```

### 5B — AJUSTE DE LIMITS (subdimensionamento)

```bash
kubectl patch deployment payments-api -n payments \
  --patch '{"spec":{"template":{"spec":{"containers":[{
    "name":"payments-api",
    "resources":{
      "requests":{"memory":"768Mi"},
      "limits":{"memory":"1536Mi"}
    }}]}}}}'

kubectl rollout status deployment/payments-api -n payments
```

### 5C — SCALE HORIZONTAL (ação imediata enquanto investiga)

```bash
kubectl scale deployment payments-api -n payments --replicas=5
kubectl get pods -n payments -w
```

---

## ETAPA 6 — Critérios de Escalação
**Tempo máximo: 3 minutos (definição)**

### Escalar para SEV1 quando:
- [ ] Latência p99 > 500ms por mais de 5 minutos (violação de SLA)
- [ ] Mais de 50% dos pods em OOMKilled simultâneo
- [ ] Rollback falhou ou demorou mais de 10 minutos
- [ ] Taxa de erros 5xx > 1% das requisições

### Acionar Time de Desenvolvimento quando:
- [ ] Memory leak confirmado via métricas (crescimento linear > 30 minutos)
- [ ] Rollback necessário (precisam investigar bug no v2.4.1)
- [ ] Causa não identificada após 30 minutos de investigação

**Canal:** #incidents-sev1 no Slack | PagerDuty: `payments-sre-oncall`

---

## ETAPA 7 — Ajustes Definitivos e Critério de Validação Pós-fix
**Tempo máximo: 5 minutos (definição) | Validação: 24–48h**

**Se subdimensionamento — novo sizing recomendado:**

```yaml
resources:
  requests:
    memory: "768Mi"
  limits:
    memory: "1536Mi"
```

**Habilitar HPA baseado em memória:**

```bash
kubectl autoscale deployment payments-api -n payments \
  --min=3 --max=8 --cpu-percent=70
```

**Alerta Prometheus — memória > 80% do limit por 5 minutos:**

```yaml
- alert: PaymentsPodHighMemory
  expr: |
    container_memory_working_set_bytes{
      namespace="payments",container="payments-api"}
    / container_spec_memory_limit_bytes{
      namespace="payments",container="payments-api"} > 0.8
  for: 5m
  labels:
    severity: warning
```

**Critério de Validação Pós-fix:**
- [ ] Nenhum OOMKilled nas próximas 24 horas
- [ ] Memória estável (sem crescimento linear nos gráficos do Prometheus)
- [ ] Latência p99 < 300ms (40% de margem do SLA)
- [ ] Revisão de código do v2.4.1 concluída pelo time de dev (se leak)

## O que funcionou

- O elemento Role com especificação técnica precisa (Kubernetes 1.29, EKS, Prometheus/Grafana,
  SEV1/SEV2) calibrou o nível de profundidade correto — sem explicações básicas desnecessárias
- O Steps forçou sequência investigativa correta (confirmar → medir → isolar → diagnosticar →
  agir → escalar → validar), que é exatamente o fluxo de um runbook de incidente real
- Todos os comandos kubectl saíram copy-paste-ready, incluindo a flag `--previous` para logs de
  containers mortos — detalhe crítico que muitos prompts genéricos perdem
- A árvore de decisão binária (leak vs subdimensionamento) com critérios objetivos eliminou
  ambiguidade: o SRE sabe exatamente qual caminho tomar em cada caso
- O critério de rollback com threshold específico (> 50MB/hora) foi gerado sem pedir
  explicitamente — surgiu da combinação de Input + Role + Steps

## O que não funcionou

- Os nomes de revisão do deployment (`--to-revision=<revision-v2.3.9>`) precisam ser
  substituídos manualmente — o modelo não conhece o histórico real do cluster
- O HPA foi configurado com `--cpu-percent=70` mas o problema é de memória; seria mais
  preciso usar custom metrics baseado em memória via metrics-server ou KEDA

## Ajustes que eu fiz

- Nenhum: o prompt foi executado uma única vez, sem refinamento adicional

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O runbook é imediatamente utilizável em produção: comandos prontos,
queries PromQL funcionais, árvore de decisão com critérios objetivos, timebox por etapa e
critérios de rollback específicos. A combinação Role (SRE sênior) + Steps (sequência de
investigação) + Expectation (entregáveis técnicos) produziu o output mais próximo possível
de um documento real de resposta a incidentes.

## O que eu faria diferente

Incluir no Input o nome real do container (além de "payments-api") e se o cluster usa
KEDA, Datadog ou outro sistema de métricas — isso permitiria que o modelo gerasse queries
e alertas específicos para a stack real, eliminando os últimos pontos de substituição manual.
