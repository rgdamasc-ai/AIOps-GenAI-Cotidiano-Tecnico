# Experimento 17 — Demo desestruturada: problema de OOMKills no Kubernetes

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pedido vago de ajuda com problema de memória no Kubernetes sem nenhum dos 4 componentes estruturais. Primeiro cenário do grupo "Demo Ops" — demonstra como a ausência de Role, Contexto, Instrução e Output gera uma lista genérica de possibilidades sem comandos concretos nem passos sequenciais.

## Prompt usado

```
Me ajuda a resolver um problema de memória no Kubernetes
```

## Resposta da IA

O modelo retornou três seções genéricas (causas, o que verificar, soluções) com listas de possibilidades — memory leak, JVM mal configurada, ausência de limits, muitos pods por node, cache sem eviction. Os comandos mencionados (kubectl top pods, kubectl describe pod, kubectl get events) são corretos mas apresentados como lista, não como sequência diagnóstica. Finalizou com perguntas de clarificação sobre o sintoma exato.

## Output completo da IA

Claro! Problemas de memória no Kubernetes são bem comuns. Aqui estão as principais causas e o que verificar:

**Possíveis causas:**
- Pods sem `resources.limits.memory` definidos — a aplicação consome memória sem restrição até o node ficar sem recursos
- Memory leak na aplicação — consumo cresce progressivamente e nunca libera
- Configuração de JVM errada em apps Java — heap não respeita os limites do container
- Muitos pods competindo pelo mesmo node sem Node Affinity ou Resource Quotas
- Dados em cache crescendo sem eviction policy

**O que verificar:**
- Use `kubectl top pods` para ver consumo atual
- Verifique se os pods estão em OOMKilled com `kubectl describe pod`
- Cheque os eventos do namespace com `kubectl get events`
- Revise os resource limits nos manifests YAML

**Possíveis soluções:**
- Defina `requests` e `limits` de memória em todos os pods
- Configure Vertical Pod Autoscaler (VPA) para ajustar automaticamente
- Revise a aplicação em busca de memory leaks
- Use LimitRange para impor limites padrão no namespace

Qual é o sintoma exato? Os pods estão sendo reiniciados (OOMKilled)? O node está ficando sem memória? Me conta mais detalhes!

## O que funcionou

- Os comandos kubectl mencionados são reais e corretos
- O modelo não inventou sintomas — pediu mais contexto antes de diagnosticar
- A lista de causas cobre os casos mais comuns de problemas de memória no K8s

## O que não funcionou

- Sem saber se é OOMKilled, pressão no node ou memory leak, nenhuma das sugestões pode ser priorizada
- Comandos apresentados como lista dispersa, não como passos sequenciais de diagnóstico
- Sem cluster, namespace, tipo de workload ou métricas, o modelo não pode propor uma hipótese
- O engenheiro em meio a um incidente recebe trabalho de triagem ao invés de um runbook

## Ajustes que eu fiz

- Nenhum — este é o prompt base sem estrutura, referência direta para o Prompt 18

## Nota (1 a 5)

**Nota:** 1/5

**Justificativa:** Em contexto de incidente, uma lista de possibilidades sem priorização é o pior resultado possível. O prompt vago em momento de pressão gera ruído em vez de direção. O Prompt 18 demonstra como a estrutura de 4 componentes transforma esse ruído em um runbook acionável.

## O que eu faria diferente

Fornecer o ambiente (EKS, GKE, on-prem), o sintoma exato (OOMKilled, node NotReady, pod em Pending), métricas observadas e o tipo de workload afetado — e pedir um runbook com passos e comandos, não uma lista de causas. Exatamente o que o Prompt 18 demonstra.
