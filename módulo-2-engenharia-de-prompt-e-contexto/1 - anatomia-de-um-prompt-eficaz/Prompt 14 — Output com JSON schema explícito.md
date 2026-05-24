# Experimento 14 — Output com JSON schema explícito

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pedido de alertas Kubernetes com schema JSON formalizado funcionando como contrato de interface. Quarto e mais avançado cenário do grupo "Output" — demonstra como um schema explícito com tipos e enums garante saída tipada e consistente, pronta para integração com PagerDuty, Slack ou dashboards de oncall.

## Prompt usado

```
Liste os 5 alertas mais comuns em clusters Kubernetes.

Formato: array JSON seguindo este schema:
{
  "alert_name": "string",
  "severity": "critical | warning | info",
  "component": "string",
  "typical_cause": "string",
  "first_action": "string (comando kubectl)"
}
Retorne apenas o JSON, sem explicações.
```

## Resposta da IA

O modelo retornou um array JSON com 5 objetos tipados: KubePodCrashLooping (critical/Pod), KubeNodeNotReady (critical/Node), KubePodOOMKilled (warning/Pod), KubeDeploymentReplicasMismatch (warning/Deployment) e KubePersistentVolumeFillingUp (warning/PersistentVolume). Todos os campos do schema foram preenchidos. O campo "severity" respeitou o enum (critical/warning/info). O campo "first_action" contém comandos kubectl reais com placeholders padronizados. Nenhum texto fora do JSON.

## Output completo da IA

```json
[
  {
    "alert_name": "KubePodCrashLooping",
    "severity": "critical",
    "component": "Pod",
    "typical_cause": "Erro na aplicação, OOM, falha no liveness probe ou imagem corrompida",
    "first_action": "kubectl describe pod <pod-name> -n <namespace>"
  },
  {
    "alert_name": "KubeNodeNotReady",
    "severity": "critical",
    "component": "Node",
    "typical_cause": "Falha no kubelet, pressão de recursos (CPU/memória/disco) ou problema de rede",
    "first_action": "kubectl describe node <node-name>"
  },
  {
    "alert_name": "KubePodOOMKilled",
    "severity": "warning",
    "component": "Pod",
    "typical_cause": "Aplicação consumindo mais memória do que o limits configurado",
    "first_action": "kubectl top pod <pod-name> -n <namespace>"
  },
  {
    "alert_name": "KubeDeploymentReplicasMismatch",
    "severity": "warning",
    "component": "Deployment",
    "typical_cause": "Pods em Pending por falta de recursos no cluster ou falha no scheduler",
    "first_action": "kubectl get events -n <namespace> --sort-by='.lastTimestamp'"
  },
  {
    "alert_name": "KubePersistentVolumeFillingUp",
    "severity": "warning",
    "component": "PersistentVolume",
    "typical_cause": "Crescimento de dados não previsto, logs acumulados ou ausência de política de retenção",
    "first_action": "kubectl exec -it <pod-name> -n <namespace> -- df -h"
  }
]
```

## O que funcionou

- Schema funcionou como contrato: todos os 5 objetos têm exatamente os campos definidos, sem extras nem faltantes
- Enum de severity respeitado — nenhum valor fora do conjunto (critical/warning/info)
- Comandos kubectl no "first_action" são reais e acionáveis, não genéricos
- Nomes de alertas no formato Prometheus/AlertManager (PascalCase com prefixo Kube) — compatíveis com stacks de monitoramento reais
- "Retorne apenas o JSON" foi respeitado — zero texto de contexto ou explicação

## O que não funcionou

- Placeholders nos comandos (`<pod-name>`, `<namespace>`) precisam de substituição manual — um campo adicional com o namespace padrão tornaria o comando executável diretamente
- Não incluiu alertas de rede (KubeNetworkErrors) que também são comuns em produção

## Ajustes que eu fiz

- Nenhum — o schema explícito foi suficiente para garantir estrutura, tipos e restrições na primeira tentativa

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Schema como contrato é a técnica mais poderosa do grupo Output: garante consistência em múltiplas execuções, elimina parsing ambíguo e produz saída diretamente integrável. Fecha o grupo de 4 cenários de Output mostrando a progressão de texto livre → tabela → JSON livre → JSON com schema.

## O que eu faria diferente

Incluir um campo "runbook_url" no schema apontando para a documentação interna do alerta, tornando o JSON diretamente utilizável como payload de notificação para PagerDuty ou Slack com link de investigação.
