---
nome: Diagnosticar Erros em Kubernetes (EKS)
descricao: Diagnostica erros de workloads em um namespace/serviço do Amazon EKS, aponta a causa raiz com evidências e sugere a correção assertiva sem aplicá-la.
versao: 1.0.0
ferramenta usada: [Claude]
modelo: [Opus 4.8]
tags: [kubernetes, eks, aws, sre, troubleshooting]
inputs:
  - nome: namespace
    descricao: Namespace do cluster EKS a ser inspecionado.
  - nome: servico
    descricao: Serviço/workload alvo dentro do namespace; se vazio, varre o namespace inteiro.
---

# Diagnosticar Erros em Kubernetes (EKS)

## Objetivo

Diagnosticar erros e degradações de workloads rodando em um cluster Amazon EKS, isolando a causa raiz com base em evidências (eventos, logs, describe) e recomendando a correção de forma assertiva — sem aplicar nenhuma mudança.

## Quando usar

- Ao investigar pods/serviços com falha (CrashLoopBackOff, ImagePullBackOff, Pending etc.) em um namespace específico.
- Quando quiser varrer um namespace inteiro do EKS em busca de qualquer erro.
- Para obter causa raiz + correção pronta (comando/manifesto) que será revisada e aplicada por um humano.
- Em triagem de incidentes onde aplicar mudanças automaticamente não é aceitável.

## Exemplo de uso

Preencher `{{namespace}}` com `pagamentos` e `{{servico}}` com `api-checkout` para focar nesse workload; deixar `{{servico}}` vazio para avaliar todo o namespace.

## Limitações conhecidas

- Depende das saídas read-only fornecidas pelo operador (`kubectl get/describe/logs`, etc.); sem evidências, só lista o que coletar.
- Não executa comandos nem acessa o cluster — apenas analisa o que recebe e recomenda.

## Testes

Este prompt acompanha o arquivo [`promptfooconfig.yaml`](./promptfooconfig.yaml), que valida com [promptfoo](https://promptfoo.dev) se o diagnóstico é eficiente e agnóstico a namespace/workload. Foi gerado a partir do prompt [gerar-config-promptfoo](../../desenvolvimento/gerar-config-promptfoo/).

**Provider:** `anthropic:messages:claude-sonnet-4-5-20250929` com `temperature: 0.0` (diagnóstico estável e reproduzível).

Cobertura das avaliações:

- **`latency`** (`defaultTest`, `threshold: 30000`) — tempo de resposta dentro de um limite útil para on-call (≤ 30s).
- **`not-regex`** (`defaultTest`) — garante a regra inviolável de não aplicar mudanças: reprova se a saída afirmar que executou (`apliquei`, `executei`, `já apliquei`, `rodei o comando`, `fiz o rollout`). Usa classes de caractere na inicial em vez da flag inline `(?i)`, não suportada pelo RegExp do Node.
- **`llm-rubric`** (por teste) — comportamento semântico: pede comandos read-only, não fabrica dados e respeita o escopo (workload específico vs. namespace inteiro vs. nomes arbitrários).
- **`contains`** (3º teste, valor `"Resumo"`) — presença do cabeçalho garantido da saída no caminho "sem evidência".

Os cenários cobrem workload específico (`production/checkout-api`), varredura de namespace inteiro com serviço vazio (`data-platform`) e nomes arbitrários (`ns-7x9-zeta`/`svc-randomico-42`, teste de agnosticismo).

Para rodar (requer `ANTHROPIC_API_KEY` no ambiente):

```bash
cd prompts/sre/diagnosticar-erros-eks && promptfoo eval
```

> **Nota sobre cobertura:** como os testes não fornecem saídas de comando, o modelo entra no modo "pedir evidências", em que o único cabeçalho garantido é `Resumo` — por isso a asserção valida só ele, e não as seções `Erros Encontrados` / `Diagnóstico Adicional Necessário`. Para exercitar o caminho de diagnóstico completo, adicione uma variável de saídas de comando ao prompt e um teste que a preencha.