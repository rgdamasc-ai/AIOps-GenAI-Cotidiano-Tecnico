# Experimento 05 — Contexto: pergunta de latência sem contexto

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pergunta genérica sobre redução de latência sem nenhum contexto técnico da aplicação. Primeiro cenário do grupo "Contexto" — demonstra como a ausência de informações sobre stack, sintomas e métricas gera uma resposta ampla e não acionável.

## Prompt usado

```
Como reduzir a latência da minha aplicação?
```

## Resposta da IA

O modelo retornou uma lista de 4 categorias (Frontend/Rede, Backend/API, Infraestrutura, Banco de dados) com múltiplas sugestões em cada uma — CDN, cache Redis, índices, connection pooling, auto-scaling, APM, read replicas, sharding. Finalizou pedindo clarificação: "Qual parte da stack está com mais problema?"

## Output completo da IA

Aqui estão as principais estratégias para reduzir latência:

**Frontend / Rede:**
- Use CDN para assets estáticos
- Ative compressão gzip/brotli
- Reduza o tamanho de bundles JS/CSS
- Implemente lazy loading e code splitting

**Backend / API:**
- Adicione cache (Redis, Memcached) para respostas frequentes
- Otimize queries no banco de dados (índices, N+1, explain analyze)
- Use connection pooling
- Considere paginação em endpoints que retornam muitos dados

**Infraestrutura:**
- Aproxime o servidor do usuário (região do cloud provider)
- Use load balancer com health checks
- Considere auto-scaling para picos de tráfego
- Monitore com APM (Datadog, New Relic, Grafana)

**Banco de dados:**
- Adicione índices nas colunas mais consultadas
- Revise queries lentas no slow query log
- Considere read replicas para consultas pesadas
- Avalie sharding se o volume for muito alto

Qual parte da stack está com mais problema? Frontend, backend ou banco?

## O que funcionou

- Cobertura abrangente de todas as camadas da stack — útil como checklist inicial
- Pergunta final de clarificação mostra que o modelo sabe que a resposta é insuficiente

## O que não funcionou

- Resposta serve para qualquer aplicação, logo não serve para nenhuma em particular
- Sem saber a stack, os sintomas e as métricas, o modelo não consegue priorizar
- Sugestões como "sharding" e "CDN" podem ser completamente irrelevantes para o caso real
- O engenheiro recebe trabalho de triagem ao invés de uma hipótese diagnóstica

## Ajustes que eu fiz

- Nenhum — este é o baseline sem contexto, referência direta para comparação com o Prompt 6

## Nota (1 a 5)

**Nota:** 2/5

**Justificativa:** A resposta é tecnicamente correta mas operacionalmente cara — exige que o engenheiro avalie manualmente quais sugestões se aplicam ao seu cenário. Sem contexto, o modelo entrega uma enciclopédia onde deveria entregar um diagnóstico.

## O que eu faria diferente

Fornecer stack, endpoint afetado, métricas de antes/depois e o momento em que o problema começou — exatamente o que o Prompt 6 demonstra.
