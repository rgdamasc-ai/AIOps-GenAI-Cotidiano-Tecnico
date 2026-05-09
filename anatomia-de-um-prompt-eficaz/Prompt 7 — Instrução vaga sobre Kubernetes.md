# Experimento 07 — Instrução vaga sobre Kubernetes

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pedido genérico de ajuda com Kubernetes sem verbo de ação claro. Primeiro cenário do grupo "Instrução" — demonstra como a ausência de um verbo específico (explicar, diagnosticar, criar, comparar) força o modelo a listar possibilidades em vez de entregar uma resposta direta.

## Prompt usado

```
Me ajuda com Kubernetes.
```

## Resposta da IA

O modelo listou 9 categorias temáticas (conceitos, diagnóstico, configuração, rede, storage, segurança, monitoramento, deploy, administração) e finalizou pedindo mais informações: "Me conta mais sobre o que você está tentando fazer ou qual problema está enfrentando!"

## Output completo da IA

Claro! Posso ajudar com Kubernetes. O que você precisa especificamente?

Alguns temas comuns com os quais posso ajudar:

- **Conceitos básicos:** Pods, Deployments, Services, ConfigMaps, Secrets
- **Diagnóstico de problemas:** pods em CrashLoopBackOff, OOMKilled, ImagePullBackOff
- **Configuração:** HPA, resource limits, liveness/readiness probes
- **Rede:** Ingress, NetworkPolicy, Service types (ClusterIP, NodePort, LoadBalancer)
- **Storage:** PersistentVolumes, PersistentVolumeClaims, StorageClasses
- **Segurança:** RBAC, ServiceAccounts, PodSecurityStandards
- **Monitoramento:** Prometheus, Grafana, kubectl top
- **Deploy:** estratégias rolling update, blue-green, canary
- **Administração:** kubeadm, EKS, GKE, AKS

Me conta mais sobre o que você está tentando fazer ou qual problema está enfrentando!

## O que funcionou

- O modelo não inventou uma tarefa — reconheceu a ambiguidade e pediu clarificação
- A lista de categorias pode funcionar como menu de opções para usuários que não sabem o que querem

## O que não funcionou

- Sem verbo de ação, o modelo não sabe se deve explicar, diagnosticar, criar documentação ou comparar ferramentas
- A resposta adiciona uma rodada de conversa desnecessária — o usuário precisa reformular antes de receber valor
- Em contextos de incidente ou pressão, essa troca de perguntas tem custo real

## Ajustes que eu fiz

- Nenhum — este é o prompt base sem instrução, referência direta para os Prompts 8, 9 e 10

## Nota (1 a 5)

**Nota:** 2/5

**Justificativa:** A resposta é defensivamente correta — o modelo não erra porque não tenta. Mas o custo é uma rodada extra de conversa e zero valor entregue na primeira resposta. Um verbo de ação resolve completamente o problema.

## O que eu faria diferente

Adicionar um verbo claro antes do substantivo: "Explique", "Compare", "Diagnostique", "Crie um runbook para". Cada um desses verbos produz um tipo de entrega completamente diferente — como demonstrado nos Prompts 8, 9 e 10.
