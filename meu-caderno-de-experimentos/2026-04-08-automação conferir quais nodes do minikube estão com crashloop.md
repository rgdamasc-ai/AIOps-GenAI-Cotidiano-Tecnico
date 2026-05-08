# Experimento 01 — Automação de monitoramento de CrashLoopBackOff no Kubernetes

**Data:** 2026-05-08
**Ferramenta usada:** Claude
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

## Contexto

Rotina manual repetitiva de verificar pods em estado `CrashLoopBackOff` no minikube ou via ArgoCD. O objetivo era eliminar esse processo manual criando um script que rodasse automaticamente via cron job e notificasse proativamente via desktop e som.

## Prompt usado

```
tenho uma rotina de ficar validando no minikube ou entrando no argoCD para validar se há pod crashloop.

Quero que escreva para mim um script ou comando para uma automação
```

*(seguido de respostas a perguntas de elicitação da IA):*

- **O que monitorar:** CrashLoopBackOff
- **Como ser notificado:** Som + notificação desktop
- **Como rodar:** Cron job

## Resposta da IA

A IA gerou dois scripts shell:

1. **`k8s-crashloop-monitor.sh`** — script principal que:
   - Itera sobre todos os namespaces (ou lista customizável via variável de ambiente `K8S_NAMESPACES`)
   - Detecta pods em `CrashLoopBackOff` via `kubectl get pods`
   - Extrai o número de restarts de cada pod problemático
   - Envia notificação desktop multiplataforma (`notify-send` no Linux, `osascript` no macOS)
   - Toca som de alerta (`paplay` → `aplay` → `afplay` → fallback para beep de terminal)
   - Registra tudo em `~/.k8s-crashloop-monitor.log` com rotação simples

2. **`install-cron.sh`** — script de instalação que:
   - Copia o monitor para `~/.local/bin/`
   - Adiciona entrada no `crontab` com intervalo configurável (padrão: 5 minutos)
   - Imprime instruções de uso e comandos úteis

## O que funcionou

- A IA pediu esclarecimentos antes de gerar o código, evitando retrabalho (perguntou sobre o que monitorar, como notificar e como executar)
- O script foi gerado já com suporte multiplataforma (Linux + macOS) sem precisar pedir
- Variáveis de ambiente para configuração (`K8S_NAMESPACES`, `K8S_SOUND_ALERT`, `K8S_MONITOR_LOG`) foram adicionadas automaticamente, tornando o script flexível
- A rotação de log foi incluída sem solicitação
- O script de instalação separado foi uma boa adição — não precisei pedir instruções de como configurar o cron

## O que não funcionou

- Nenhum problema significativo neste experimento

## Ajustes que eu fiz

- Nenhum ajuste necessário; o script funcionou conforme esperado na primeira geração

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** A IA transformou uma descrição vaga ("automatizar validação de crashloop") em dois scripts prontos para uso, com tratamento de erros, suporte multiplataforma, configuração via variáveis de ambiente e instruções claras — tudo sem precisar de iterações adicionais.

## O que eu faria diferente

Poderia ter especificado de antemão o intervalo desejado do cron job e se queria suporte ao ArgoCD além do kubectl direto. Para um próximo experimento, vale explorar integração com Slack webhook para times maiores, onde notificação só no desktop de uma pessoa pode não ser suficiente.