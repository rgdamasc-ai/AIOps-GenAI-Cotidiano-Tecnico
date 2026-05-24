# Zero-Shot Prompting

---

## Prompt 1 — Organização e priorização de tarefas

Prompt do dia a dia usado no Passo 1 da demo. Mostra que o modelo entende prioridade por contexto (palavras como "amanhã" e "vence hoje") sem precisar de exemplos — esse é o poder do Zero-Shot em tarefas conhecidas.

```
Organize a lista abaixo em uma tabela com colunas: tarefa, categoria, prioridade (alta/média/baixa).

- Comprar presente pro aniversário do João que é amanhã
- Responder e-mail do contador sobre imposto de renda
- Trocar lâmpada da cozinha
- Marcar dentista
- Pagar fatura do cartão que vence hoje
- Assistir episódio novo da série
```

---

## Prompt 2 — Classificação de severidade de alerta

Prompt técnico do Passo 2 da demo. Pede classificação de um alerta de monitoramento (Pod em CrashLoopBackOff) em JSON estruturado. Demonstra Zero-Shot funcionando bem para tarefas que o modelo conhece do treinamento.

```
Classifique o alerta abaixo como critical, high, medium ou low. Responda em JSON com os campos: severity, justification e recommended_action.

Alerta: "Pod CrashLoopBackOff no deployment payment-service no namespace production. Restarts: 15 nos últimos 30 minutos."
```

---

## Prompt 3 — Geração de script Bash de automação

Prompt técnico do Passo 3 da demo. Pede a criação de um script Bash completo a partir de uma descrição em linguagem natural. Mostra que Zero-Shot funciona bem para criação de scripts quando a tarefa é clara e usa tecnologias bem documentadas.

```
Crie um script em Bash que verifique todos os pods em estado de erro no namespace production de um cluster Kubernetes. O script deve listar o nome do pod, status e número de restarts, e enviar um alerta no stdout se algum pod tiver mais de 5 restarts.
```

---

## Caderno de Experimentos

---

### Experimento 01 — Organização e priorização de tarefas

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Teste de zero-shot para organizar lista de tarefas cotidianas com priorização automática inferida do contexto temporal implícito no texto ("vence hoje", "é amanhã").

#### Prompt usado

```
Organize a lista abaixo em uma tabela com colunas: tarefa, categoria, prioridade (alta/média/baixa).

- Comprar presente pro aniversário do João que é amanhã
- Responder e-mail do contador sobre imposto de renda
- Trocar lâmpada da cozinha
- Marcar dentista
- Pagar fatura do cartão que vence hoje
- Assistir episódio novo da série
```

#### Resposta da IA

Tabela Markdown gerada corretamente, com prioridade Alta aplicada às duas tarefas com urgência temporal explícita, sem exemplos fornecidos.

#### Output completo da IA

| Tarefa | Categoria | Prioridade |
|--------|-----------|------------|
| Pagar fatura do cartão que vence hoje | Financeiro | Alta |
| Comprar presente pro aniversário do João que é amanhã | Pessoal | Alta |
| Responder e-mail do contador sobre imposto de renda | Financeiro | Média |
| Marcar dentista | Saúde | Média |
| Trocar lâmpada da cozinha | Casa | Baixa |
| Assistir episódio novo da série | Lazer | Baixa |

#### O que funcionou

- Identificou corretamente urgência implícita em "vence hoje" e "amanhã" → Alta
- Categorias coerentes sem precisar de exemplos
- Formato Markdown entregue diretamente, pronto para uso

#### O que não funcionou

- Categoria do e-mail do contador poderia ser "Fiscal/Tributário" em vez de "Financeiro"
- Não separou o critério de urgência vs. importância explicitamente

#### Ajustes que eu fiz

- Nenhum ajuste necessário para a demo. Em contexto real, adicionar "priorize por urgência primeiro, depois por impacto no negócio".

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Zero-Shot entregou exatamente o esperado sem nenhum exemplo. Demonstra que a técnica funciona bem quando o padrão da tarefa é amplamente conhecido pelo modelo.

#### O que eu faria diferente

Adicionar coluna "prazo" e instruir o modelo a usar a matriz Urgência × Importância (Eisenhower) para tornar a tabela mais acionável em contexto profissional.

---

### Experimento 02 — Classificação de severidade de alerta

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de zero-shot para classificar a severidade de um alerta de monitoramento Kubernetes em JSON estruturado, sem exemplos de classificação fornecidos.

#### Prompt usado

```
Classifique o alerta abaixo como critical, high, medium ou low. Responda em JSON com os campos: severity, justification e recommended_action.

Alerta: "Pod CrashLoopBackOff no deployment payment-service no namespace production. Restarts: 15 nos últimos 30 minutos."
```

#### Resposta da IA

Classificação `critical` com justificativa orientada ao impacto do serviço de pagamento e ações recomendadas ordenadas por prioridade de investigação.

#### Output completo da IA

```json
{
  "severity": "critical",
  "justification": "O deployment payment-service está em CrashLoopBackOff no namespace production com 15 reinicializações em 30 minutos. Serviço de pagamento em produção com instabilidade recorrente impacta diretamente a disponibilidade e transações financeiras dos usuários.",
  "recommended_action": "1. Executar 'kubectl logs payment-service -n production --previous' para verificar os logs antes do crash. 2. Executar 'kubectl describe pod -n production' para identificar OOMKilled, falhas de configuração ou dependências ausentes. 3. Verificar se houve deploy recente e considerar rollback imediato. 4. Acionar equipe on-call via PagerDuty imediatamente."
}
```

#### O que funcionou

- Classificação `critical` correta dado o contexto (serviço de pagamento + produção + 15 restarts)
- JSON válido com os três campos solicitados
- Ações recomendadas práticas e em ordem de prioridade de investigação

#### O que não funcionou

- `recommended_action` como string com lista numerada — em AIOps real, deveria ser array JSON para parsing automatizado
- Ausência de campos como `runbook_url` e `team_responsible`, úteis em integração com Ops platforms

#### Ajustes que eu fiz

- Em uso produtivo: especificar "retorne recommended_action como array de strings" e adicionar campos `runbook_url` e `escalation_policy` ao schema solicitado.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Zero-Shot classificou corretamente e entregou JSON estruturado sem exemplos. Adequado para integração com pipelines de alerting e rotas de escalação automática.

#### O que eu faria diferente

Evoluir para Few-Shot para padronizar o schema exato do JSON (array em `recommended_action`) e calibrar o threshold entre `critical` e `high` com exemplos concretos de cada nível.

---

### Experimento 03 — Geração de script Bash de automação

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Teste de zero-shot para gerar script Bash completo de verificação de pods Kubernetes com alerta em stdout, a partir de descrição em linguagem natural, sem fornecer exemplos de código.

#### Prompt usado

```
Crie um script em Bash que verifique todos os pods em estado de erro no namespace production de um cluster Kubernetes. O script deve listar o nome do pod, status e número de restarts, e enviar um alerta no stdout se algum pod tiver mais de 5 restarts.
```

#### Resposta da IA

Script Bash com shebang, variáveis configuráveis, loop sobre output do `kubectl` com parsing via `awk` e lógica de threshold para alerta.

#### Output completo da IA

```bash
#!/bin/bash

NAMESPACE="production"
RESTART_THRESHOLD=5

echo "=== Verificação de Pods em Erro no namespace: $NAMESPACE ==="
echo ""

kubectl get pods -n "$NAMESPACE" --no-headers | while read -r line; do
    POD_NAME=$(echo "$line" | awk '{print $1}')
    STATUS=$(echo "$line" | awk '{print $3}')
    RESTARTS=$(echo "$line" | awk '{print $4}')

    if [[ "$STATUS" != "Running" ]] || [[ "$RESTARTS" -gt "$RESTART_THRESHOLD" ]]; then
        echo "Pod: $POD_NAME | Status: $STATUS | Restarts: $RESTARTS"

        if [[ "$RESTARTS" -gt "$RESTART_THRESHOLD" ]]; then
            echo "⚠️  ALERTA: Pod $POD_NAME tem $RESTARTS reinicializações (threshold: $RESTART_THRESHOLD)"
        fi
    fi
done
```

#### O que funcionou

- Estrutura limpa com variáveis configuráveis no topo (`NAMESPACE`, `RESTART_THRESHOLD`)
- Detecção correta de pods fora de `Running` e com restarts acima do threshold
- `--no-headers` facilita o parsing com `awk`

#### O que não funcionou

- Comparação inteira com `[[ "$RESTARTS" -gt ... ]]` falha em pods com múltiplos containers (campo `READY` mostra `N/M`, deslocando colunas do `awk`)
- Sem tratamento de erro para `kubectl` indisponível ou cluster inacessível

#### Ajustes que eu fiz

- Adicionar verificação de conectividade: `kubectl cluster-info > /dev/null 2>&1 || { echo "Cluster inacessível"; exit 1; }`
- Usar `kubectl get pods -o json | jq` para extração robusta em pods com múltiplos containers

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** Script funcional para o caso simples (pod com 1 container). Edge cases com múltiplos containers exigem ajuste no prompt ou iteração. Zero-Shot entregou a estrutura correta, mas não é production-ready sem refinamento.

#### O que eu faria diferente

Adicionar ao prompt: "considere que pods podem ter múltiplos containers, use jsonpath ou jq para extração robusta dos dados" e "inclua tratamento de erro se o cluster não estiver acessível". Isso tornaria o resultado diretamente utilizável em produção.
