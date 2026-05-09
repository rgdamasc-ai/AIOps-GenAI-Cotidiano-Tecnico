# Experimento 06 — Contexto: pergunta de latência com contexto detalhado

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma pergunta de latência do Prompt 5, agora enriquecida com stack (Node.js + ECS Fargate + PostgreSQL/RDS), endpoint afetado (/pedidos), métricas (200ms → 1.2s), indicador de infraestrutura (RDS 85% CPU) e evento causal (deploy de sexta, apenas mudança de código). Demonstra o impacto direto do componente Contexto na qualidade diagnóstica da resposta.

## Prompt usado

```
Temos uma API em Node.js rodando no ECS Fargate, com banco PostgreSQL no RDS.
A latência do endpoint /pedidos subiu de 200ms para 1.2s após o deploy de sexta.
O RDS está em 85% de CPU. Não houve mudança de infra, apenas código.

Como reduzir a latência?
```

## Resposta da IA

O modelo entregou um diagnóstico direcional: identificou a causa mais provável como query problemática introduzida no deploy (full table scan, N+1 ou missing LIMIT), priorizou hipóteses por probabilidade e forneceu comandos SQL prontos para execução (pg_stat_statements, EXPLAIN ANALYZE, pg_stat_activity). Incluiu instrução de índice com CREATE INDEX CONCURRENTLY e recomendação de rollback como contingência.

## Output completo da IA

Com base no que você descreveu, o diagnóstico mais provável é uma **query problemática introduzida no deploy de sexta**. RDS em 85% de CPU com latência 6x maior e apenas mudança de código aponta quase certamente para o banco.

**Hipóteses prioritárias (por ordem de probabilidade):**

1. **Query sem índice ou com full table scan** — alguma query nova ou modificada no `/pedidos` está varrendo a tabela inteira
2. **Problema de N+1** — o código novo está fazendo uma query por item ao invés de um JOIN ou IN clause
3. **Missing LIMIT em resultado grande** — endpoint retornando volume muito maior que antes

**Passos imediatos (próximos 30 minutos):**

```sql
-- 1. Identifique as queries mais lentas no RDS
SELECT query, mean_exec_time, calls, total_exec_time
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;

-- 2. Rode EXPLAIN ANALYZE na query suspeita
EXPLAIN (ANALYZE, BUFFERS) SELECT ... FROM pedidos WHERE ...;

-- 3. Verifique locks ativos
SELECT * FROM pg_stat_activity WHERE wait_event_type = 'Lock';
```

**Se confirmar query lenta:**
- Adicione índice na coluna do WHERE/JOIN: `CREATE INDEX CONCURRENTLY idx_pedidos_... ON pedidos(...);`
- Se for N+1, corrija o ORM para usar eager loading ou reescreva a query com JOIN

**Rollback imediato se necessário:**
Reverter o deploy de sexta enquanto investiga é a ação mais segura se o RDS continuar subindo.

## O que funcionou

- Diagnóstico imediato e priorizado — sem listar CDN ou sharding que seriam irrelevantes
- Comandos SQL executáveis colados diretamente na resposta, prontos para uso
- Menção ao rollback como ação de contenção paralela à investigação
- Raciocínio causal explícito: "RDS 85% CPU + apenas mudança de código = query problemática"

## O que não funcionou

- Poderia ter pedido o output do EXPLAIN ANALYZE para refinar ainda mais o diagnóstico
- Não mencionou verificar o changelog do deploy para identificar qual arquivo/função mudou

## Ajustes que eu fiz

- Nenhum ajuste necessário — o contexto fornecido foi suficiente para uma resposta acionável
- O prompt combina stack + sintoma + métrica + evento causal: o conjunto mínimo viável de contexto

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Contraste com o Prompt 5 é o ponto central: mesma pergunta, resposta 6x mais útil. O contexto transformou uma lista genérica em um runbook de diagnóstico com comandos reais e hipóteses priorizadas.

## O que eu faria diferente

Adicionar o trecho de código que mudou no deploy como contexto adicional. Com o diff do endpoint /pedidos em mãos, o modelo poderia apontar a linha exata com o problema em vez de fornecer hipóteses.
