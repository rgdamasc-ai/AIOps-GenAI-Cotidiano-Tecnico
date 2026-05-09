# Experimento 08 — Instrução específica: explicar diferenças entre recursos

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma área do Prompt 7 (Kubernetes), com verbo de ação "Explique" e objetos específicos (Deployment, StatefulSet, DaemonSet). Demonstra como um único verbo direcional transforma um pedido vago em uma entrega estruturada e imediatamente utilizável.

## Prompt usado

```
Explique a diferença entre Deployment, StatefulSet e DaemonSet no Kubernetes.
```

## Resposta da IA

O modelo entregou três blocos conceituais (um por recurso) com definição, casos de uso ideais e características-chave (identidade de Pod, ordem de inicialização, relação com PVC, escopo por node). Finalizou com uma tabela comparativa resumindo Estado | Identidade | Escopo para os três controladores.

## Output completo da IA

Os três são controladores de workload no Kubernetes, mas servem a propósitos bem distintos:

**Deployment**
O controlador mais comum. Gerencia Pods stateless — cada réplica é intercambiável e pode ser substituída por qualquer outra. O Kubernetes pode matar e recriar Pods em qualquer ordem. Ideal para APIs REST, frontends, workers sem estado.
- Suporta rolling update e rollback
- Réplicas não têm identidade fixa (Pod names: `app-7d4b9c-xkq2p`)
- Não garante ordem de inicialização

**StatefulSet**
Projetado para workloads com estado persistente. Cada Pod tem identidade fixa e estável (names: `postgres-0`, `postgres-1`), volume dedicado e ordem garantida de criação/deleção. O Pod `postgres-1` só sobe após `postgres-0` estar Ready.
- Ideal para bancos de dados (PostgreSQL, MySQL), filas (Kafka, RabbitMQ), sistemas distribuídos (ZooKeeper, Elasticsearch)
- Cada réplica tem seu próprio PersistentVolumeClaim
- Rollout e rollback respeitam a ordem numérica

**DaemonSet**
Garante que exatamente um Pod rode em cada node do cluster (ou em nodes selecionados). Quando um novo node entra no cluster, o DaemonSet automaticamente agenda um Pod nele.
- Ideal para agentes de infraestrutura: coletores de logs (Fluentd, Filebeat), agentes de monitoramento (Datadog agent, Prometheus node-exporter), proxies de rede (Calico, Cilium)
- Não tem conceito de réplicas — a quantidade de Pods é igual ao número de nodes

**Resumo rápido:**
| Controlador | Estado | Identidade | Escopo |
|---|---|---|---|
| Deployment | Stateless | Aleatória | N réplicas |
| StatefulSet | Stateful | Fixa e ordenada | N réplicas com PVC |
| DaemonSet | Qualquer | Por node | 1 por node |

## O que funcionou

- Verbo "Explique" definiu claramente o tipo de entrega: conceitual, não diagnóstico nem operacional
- Tabela comparativa ao final — entregue sem solicitar, reconhecendo que o contexto era de comparação
- Casos de uso reais (PostgreSQL, Kafka, Fluentd, Datadog) tornam a explicação concreta e aplicável
- Nenhuma pergunta de clarificação — o modelo sabia exatamente o que entregar

## O que não funcionou

- Para uma audiência avançada, poderia aprofundar edge cases (ex: StatefulSet com `podManagementPolicy: Parallel`)
- Não mencionou ReplicaSet como contexto histórico do Deployment

## Ajustes que eu fiz

- Apenas adicionado o verbo "Explique" e os três objetos específicos em relação ao Prompt 7
- Nenhuma outra instrução foi necessária para obter uma resposta estruturada

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O verbo "Explique" + objetos específicos eliminaram toda a ambiguidade e entregaram uma resposta pronta para uso em documentação ou onboarding. Contraste direto com o Prompt 7 demonstra o valor da instrução precisa.

## O que eu faria diferente

Adicionar o contexto do público-alvo ("para um desenvolvedor que nunca usou Kubernetes" vs. "para um SRE migrando de VMs") para calibrar a profundidade técnica da explicação.
