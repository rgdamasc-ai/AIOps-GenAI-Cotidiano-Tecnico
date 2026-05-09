# Experimento 18 — Demo estruturada: runbook de OOMKills com 4 componentes

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesmo problema do Prompt 17 (OOMKills no Kubernetes), reestruturado com os 4 componentes completos: Role (SRE senior, 10 anos de K8s e AWS), Contexto (cluster EKS com 50 nodes, OOMKills frequentes nos pods de API), Instrução (crie um runbook de troubleshooting) e Output (tabela Markdown com 4 colunas, máximo 10 passos, comandos kubectl reais). Demonstração final e mais técnica do módulo.

## Prompt usado

```
# Role
Você é um SRE senior com 10 anos de experiência em Kubernetes e AWS.

# Contexto
Temos um cluster EKS com 50 nodes e estamos tendo OOMKills frequentes nos pods de API.

# Instrução
Crie um runbook de troubleshooting para diagnosticar e resolver OOMKills.

# Output
Formato: tabela Markdown com colunas Passo | Comando | O que verificar | Ação.
Máximo 10 passos. Use apenas comandos kubectl reais, não genéricos.
```

## Resposta da IA

O modelo entregou uma tabela de 10 passos sequenciais, cada um com comando kubectl real, critério de verificação e ação correspondente. O fluxo segue uma lógica diagnóstica progressiva: identificação (passos 1-2) → quantificação (3-5) → análise causal (6-7) → remediação (8-10). Todos os comandos são kubectl reais com flags específicas (--sort-by, jsonpath, --previous, grep filtrado). Nenhum texto fora da tabela.

## Output completo da IA

| Passo | Comando | O que verificar | Ação |
|---|---|---|---|
| 1 | `kubectl get pods -A \| grep OOMKilled` | Quais pods estão em OOMKilled e em qual namespace | Identificar o escopo do problema |
| 2 | `kubectl describe pod <pod> -n <ns> \| grep -A5 "Last State"` | Exit code 137 confirma OOMKill; verificar tempo de vida do pod | Confirmar que é OOMKill e não outro tipo de falha |
| 3 | `kubectl top pods -n <ns> --sort-by=memory` | Pods com maior consumo de memória no namespace afetado | Identificar quais pods são os maiores consumidores |
| 4 | `kubectl get pod <pod> -n <ns> -o jsonpath='{.spec.containers[*].resources}'` | Verificar se limits de memória estão definidos e quais são os valores | Se não houver limits, definir imediatamente |
| 5 | `kubectl top nodes` | Nodes com pressão de memória (>85% de uso) | Se múltiplos nodes sobrecarregados, avaliar scale-out do cluster |
| 6 | `kubectl get events -n <ns> --sort-by='.lastTimestamp' \| grep OOM` | Frequência e padrão temporal dos OOMKills (horário de pico?) | Correlacionar com deploys recentes ou variação de tráfego |
| 7 | `kubectl logs <pod> -n <ns> --previous \| tail -100` | Logs antes do crash — consumo crescente, GC excessivo, erros de alocação | Identificar se é memory leak ou pico de carga pontual |
| 8 | `kubectl get vpa -n <ns>` | Se VPA está configurado e quais são as recomendações atuais | Aplicar recomendações do VPA ou ajustar limits manualmente |
| 9 | `kubectl set resources deployment <deploy> -n <ns> --limits=memory=512Mi` | Aplicar novo limite de memória e monitorar por 10 minutos | Aumentar limits se a aplicação legitimamente precisa de mais memória |
| 10 | `kubectl rollout restart deployment <deploy> -n <ns>` | Pods reiniciam com nova configuração de recursos | Confirmar que OOMKills cessaram após ajuste |

## O que funcionou

- Runbook sequencial e acionável — pode ser executado diretamente durante um incidente
- Comandos com flags específicas (ex: `-o jsonpath='{.spec.containers[*].resources}'`) são mais precisos do que versões genéricas
- Distinção entre memory leak e pico de carga (passo 7) é um diagnóstico diferencial real que júniores frequentemente pulam
- VPA como alternativa à correção manual (passo 8) é uma prática avançada que o role de SRE senior justifica incluir
- Limite de 10 passos forçou priorização — o modelo não diluiu o runbook com passos redundantes

## O que não funcionou

- Placeholders (`<pod>`, `<ns>`, `<deploy>`) precisam ser substituídos manualmente — em um runbook de produção, o namespace dos pods de API deveria ser fixado
- Não incluiu um passo de alerting/monitoramento pós-remediação para confirmar que o problema cessou

## Ajustes que eu fiz

- Nenhum — o prompt estruturado entregou o runbook completo na primeira tentativa

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Contraste máximo com o Prompt 17: de lista dispersa de possibilidades para runbook sequencial de 10 passos com comandos reais, critérios de verificação e ações. Os 4 componentes transformaram uma resposta de triagem em uma ferramenta operacional pronta para uso em incidente. Encerramento perfeito do módulo de anatomia de prompt.

## O que eu faria diferente

Adicionar uma coluna "Tempo esperado" na tabela para que o runbook sirva também como SLO interno de diagnóstico (ex: passos 1-4 em menos de 5 minutos, remediação em menos de 15). Tornaria o documento auditável em revisões pós-incidente.
