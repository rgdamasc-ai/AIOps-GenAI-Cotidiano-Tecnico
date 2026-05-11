# Framework R.I.S.E.

O framework **R.I.S.E.** estrutura prompts em quatro elementos que garantem respostas completas, no formato certo e com o tom esperado:

| Elemento | Descrição |
|---|---|
| **R**ole | Define o papel/persona que o modelo deve assumir |
| **I**nput | Fornece os dados, contexto e restrições do problema |
| **S**teps | Estabelece a sequência de raciocínio ou etapas a seguir |
| **E**xpectation | Especifica o formato, entregáveis e critérios de qualidade da resposta |

---

## Prompt 1 — Mudança de cidade com checklist completo

**Cenário:** Planejamento de mudança interestadual de família de 4 pessoas (SP → Florianópolis). Steps definem a sequência (custo de vida, bairros, escolas, custos, cronograma, checklist) e Expectation amarra o formato.

```markdown
# Role
Você é um consultor de relocação com 15 anos de experiência em mudanças interestaduais no Brasil, especializado em famílias com crianças.

# Input
- Família: casal + 2 filhos (8 e 12 anos, em idade escolar)
- Origem: São Paulo/SP (apartamento alugado, 90m²)
- Destino: Florianópolis/SC (bairro a definir)
- Orçamento total para mudança: R$ 15.000
- Prazo: 60 dias a partir de hoje
- Ambos os pais trabalham remoto
- Prioridades: qualidade de vida, boas escolas, custo de vida menor

# Steps
1. Analisar custo de vida comparativo SP vs Florianópolis (moradia, alimentação, transporte, educação)
2. Recomendar 3 bairros em Florianópolis que atendam as prioridades da família
3. Listar escolas particulares e públicas bem avaliadas nos bairros recomendados
4. Estimar custos detalhados da mudança (frete, viagem, depósito do aluguel, primeiros meses)
5. Montar cronograma de 60 dias com marcos semanais
6. Criar checklist de documentação e providências (transferência escolar, endereço, contas)

# Expectation
Espero um plano de ação completo em formato de documento estruturado com:
- Tabela comparativa de custos SP vs Florianópolis
- Ranking dos 3 bairros com prós e contras
- Cronograma visual com marcos semanais e responsáveis
- Checklist final com critério de conclusão para cada item
- Orçamento detalhado que não ultrapasse R$ 15.000
```

---

## Prompt 2 — Runbook de resposta a incidente OOMKilled

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
