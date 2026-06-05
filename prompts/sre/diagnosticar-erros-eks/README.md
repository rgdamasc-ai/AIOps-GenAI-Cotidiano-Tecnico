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