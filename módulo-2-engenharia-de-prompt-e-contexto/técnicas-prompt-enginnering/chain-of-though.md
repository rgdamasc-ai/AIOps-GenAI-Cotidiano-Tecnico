# Chain-of-Thought Prompting

---

## Prompt 1 — Lista de churrasco (sem CoT)

Prompt baseline da demo não-técnica. Pede a lista direto, sem instruir o modelo a raciocinar. Resultado: lista plausível mas genérica, sem cálculo de quantidades nem justificativa.

```
Monte uma lista de compras para um churrasco para 15 pessoas, incluindo 2 crianças. Preciso comprar tudo, incluindo descartáveis e bebidas.
```

---

## Prompt 2 — Lista de churrasco (com CoT estruturado)

Mesmo contexto do Prompt 1, mas com 5 etapas de raciocínio explícito. Resultado: o modelo calcula quantidades por perfil (adulto/criança), separa categorias e entrega lista justificada. Demonstra CoT estruturado — não é "pense passo a passo" genérico, é dizer **quais etapas** seguir.

```
Monte uma lista de compras para um churrasco para 15 pessoas, incluindo 2 crianças. Preciso comprar tudo, incluindo descartáveis e bebidas.

Antes de montar a lista, raciocine passo a passo:
1. Calcule a quantidade de carne por pessoa considerando adultos e crianças
2. Separe as carnes por bovinos, suínos e aves
3. Calcule bebidas separando adultos e crianças, alcoólicos e não alcoólicos
4. Liste os itens complementares (descartáveis, carvão, temperos)
5. Monte a lista final com quantidades calculadas
```

---

## Prompt 3 — Análise de logs de incidente (sem CoT)

Cenário técnico baseline: pedido de análise direta da causa raiz a partir dos logs. Sem CoT, o modelo identifica o problema ("connection pool esgotado") mas pula direto para a conclusão sem mostrar o caminho.

```
Analise os logs abaixo de um incidente em produção e identifique a causa raiz.

[logs do incidente — ver arquivo "Logs do incidente" abaixo]
```

---

## Prompt 4 — Análise de logs de incidente (com CoT estruturado)

Mesmo contexto do Prompt 3, com 5 etapas de raciocínio estruturado. Resultado: o modelo reconstrói a narrativa temporal, calcula recursos envolvidos e explica **por que** o sistema falhou, com auditabilidade completa.

```
Analise os logs abaixo de um incidente em produção e identifique a causa raiz.

Antes de concluir, raciocine passo a passo:
1. Identifique o evento inicial que desencadeou o incidente
2. Correlacione os timestamps para reconstruir a sequência de falha
3. Calcule os recursos envolvidos (conexões, réplicas, limites)
4. Explique por que o sistema falhou com base nos números
5. Apresente a causa raiz e o que a provocou

[logs do incidente — ver arquivo "Logs do incidente" abaixo]
```

---

## Prompt 5 — Post-mortem encadeado (CoT em cadeia)

Prompt do Passo 3 da demo. Pega o output da análise anterior e gera um post-mortem estruturado. Demonstra **CoT encadeado** — o raciocínio de uma etapa alimenta o próximo artefato sem retrabalho manual.

```
Com base na análise de causa raiz acima, gere um documento de post-mortem estruturado. Raciocine passo a passo para:

1. Definir a severidade e o impacto com base nos dados dos logs
2. Montar a timeline com os marcos críticos
3. Derivar ações corretivas a partir da causa raiz identificada
4. Sugerir ações preventivas para evitar recorrência

Use o formato:
- Título do incidente
- Data, duração, severidade
- Resumo executivo
- Timeline
- Causa raiz
- Ações corretivas (com responsável sugerido e prazo)
- Ações preventivas
```

---

## Logs do incidente (payment-service)

Bloco de logs fictícios usados nos Prompts 3 e 4. Cole junto com o prompt na hora de reproduzir a demo.

```
[2025-11-15 14:30:01] deploy-controller: Starting rollout for deployment payment-service v2.3.1
[2025-11-15 14:30:15] deploy-controller: Rollout completed — 3/3 replicas updated
[2025-11-15 14:31:02] payment-service-pod-7x9k2: INFO - Connecting to PostgreSQL at rds-payment.internal:5432
[2025-11-15 14:31:03] payment-service-pod-7x9k2: INFO - Connection pool initialized: max_connections=20
[2025-11-15 14:31:10] payment-service-pod-a3m8f: INFO - Connection pool initialized: max_connections=20
[2025-11-15 14:31:12] payment-service-pod-q1n5p: INFO - Connection pool initialized: max_connections=20
[2025-11-15 14:32:00] alb-ingress: 200 OK - /api/payments (23ms)
[2025-11-15 14:35:22] payment-service-pod-7x9k2: WARN - Connection pool usage: 18/20 (90%)
[2025-11-15 14:36:45] payment-service-pod-a3m8f: WARN - Connection pool usage: 19/20 (95%)
[2025-11-15 14:37:01] payment-service-pod-q1n5p: ERROR - Cannot acquire connection from pool: timeout after 5000ms
[2025-11-15 14:37:02] alb-ingress: 500 Internal Server Error - /api/payments
[2025-11-15 14:37:15] payment-service-pod-7x9k2: ERROR - Cannot acquire connection from pool: timeout after 5000ms
[2025-11-15 14:37:16] payment-service-pod-a3m8f: ERROR - Cannot acquire connection from pool: timeout after 5000ms
[2025-11-15 14:37:30] cloudwatch-alarm: ALARM - payment-service error rate > 50% (current: 87%)
[2025-11-15 14:38:00] rds-monitoring: INFO - Active connections: 60/100 — CPU: 92%
[2025-11-15 14:40:00] oncall-pagerduty: P1 incident opened — payment-service unavailable
```

---

## Caderno de Experimentos

---

### Experimento 01 — Lista de churrasco (sem CoT)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Baseline da técnica CoT: lista de compras para churrasco solicitada diretamente, sem instruir o modelo a raciocinar. Serve como comparativo para o Experimento 02.

#### Prompt usado

```
Monte uma lista de compras para um churrasco para 15 pessoas, incluindo 2 crianças. Preciso comprar tudo, incluindo descartáveis e bebidas.
```

#### Resposta da IA

Lista gerada de forma genérica, sem cálculo de quantidades por perfil, sem separação alcoólico/não-alcoólico e sem justificativa de quantidades.

#### Output completo da IA

**Carnes:**
- Picanha (3kg)
- Fraldinha (2kg)
- Linguiça calabresa (2kg)
- Frango (1,5kg)

**Bebidas:**
- Cerveja (3 caixas de 12)
- Refrigerante (4 garrafas 2L)
- Suco de caixinha (6 unid)
- Água mineral (2 caixas de 12)

**Descartáveis:**
- Pratos (30 unid)
- Copos (40 unid)
- Garfos e facas (30 de cada)
- Guardanapos (2 pacotes)

**Complementos:**
- Pão de alho (2 pacotes)
- Vinagrete (ingredientes)
- Carvão (2 sacos de 5kg)
- Sal grosso, temperos

#### O que funcionou

- Lista cobrindo as categorias principais sem ser solicitado
- Itens de descartáveis completos

#### O que não funcionou

- Quantidades genéricas sem cálculo por pessoa/perfil (adulto vs criança)
- Sem separação clara entre bebidas alcoólicas e não-alcoólicas
- Nenhuma justificativa para as quantidades escolhidas
- Cerveja pode ser insuficiente ou excessiva dependendo do perfil do grupo

#### Ajustes que eu fiz

- Nenhum. Este prompt é o baseline intencional para demonstrar a diferença com CoT estruturado.

#### Nota (1 a 5)

**Nota:** 3/5

**Justificativa:** Resultado plausível mas não confiável para compra real. Sem cálculo explícito de quantidades, é impossível auditar se a lista está correta ou ajustá-la para outros contextos.

#### O que eu faria diferente

Usar o Experimento 02 (CoT estruturado) para qualquer lista de compras real que envolva cálculo de quantidades por perfil ou restrições específicas.

---

### Experimento 02 — Lista de churrasco (com CoT estruturado)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Mesmo contexto do Experimento 01, mas com 5 etapas de raciocínio explícito. Demonstra CoT estruturado — não é "pense passo a passo" genérico, é definir quais etapas seguir.

#### Prompt usado

```
Monte uma lista de compras para um churrasco para 15 pessoas, incluindo 2 crianças. Preciso comprar tudo, incluindo descartáveis e bebidas.

Antes de montar a lista, raciocine passo a passo:
1. Calcule a quantidade de carne por pessoa considerando adultos e crianças
2. Separe as carnes por bovinos, suínos e aves
3. Calcule bebidas separando adultos e crianças, alcoólicos e não alcoólicos
4. Liste os itens complementares (descartáveis, carvão, temperos)
5. Monte a lista final com quantidades calculadas
```

#### Resposta da IA

Raciocínio em 5 etapas explícitas com cálculo de quantidades por perfil, separação alcoólico/não-alcoólico e lista final auditável.

#### Output completo da IA

**Etapa 1 — Cálculo de carne:**
- 13 adultos × 400g = 5,2kg
- 2 crianças × 150g = 0,3kg
- Total: ~5,5kg de carne

**Etapa 2 — Separação por tipo:**
- Bovinos: picanha 2kg + fraldinha 1,5kg = 3,5kg
- Suínos: linguiça calabresa 1kg + costelinha 0,5kg = 1,5kg
- Aves: frango temperado 0,5kg

**Etapa 3 — Bebidas:**
- Adultos (13): cerveja ~4 long necks/pessoa = ~52 unid (4-5 caixas); vinho opcional
- Crianças (2): suco de caixinha 6 unid + refrigerante 1 garrafa 2L
- Todos: água mineral 24 garrafas (2 caixas de 12)

**Etapa 4 — Complementos:**
- Descartáveis: pratos 30, copos 50, talheres 30, guardanapos 3 pacotes, papel toalha 2 rolos
- Carvão: 2 sacos de 5kg (10kg total para 4-5h de churrasqueira)
- Temperos: sal grosso 1kg, azeite, limão 6 unid, alho

**Etapa 5 — Lista final consolidada:**
*(lista organizada por categoria com quantidades calculadas e justificadas acima)*

#### O que funcionou

- Quantidades calculadas e auditáveis por etapa
- Separação explícita adultos/crianças na carne e nas bebidas
- Separação alcoólico/não-alcoólico tornando a lista adaptável a contextos variados
- Cada etapa pode ser ajustada independentemente

#### O que não funcionou

- Output mais longo — não ideal para uso rápido sem contexto de aprendizado
- Não perguntou sobre restrições alimentares (vegetarianos, alérgicos)

#### Ajustes que eu fiz

- Nenhum para esta demo. Em uso real, adicionar "considere que 2 pessoas são vegetarianas" para testar a adaptabilidade do CoT.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** CoT estruturado entregou lista auditável com raciocínio transparente. A diferença em relação ao Experimento 01 é evidente: as quantidades são justificadas e ajustáveis sem reescrever o prompt.

#### O que eu faria diferente

Adicionar uma Etapa 0: "identifique restrições alimentares e preferências antes de calcular" para tornar o raciocínio ainda mais robusto para grupos heterogêneos.

---

### Experimento 03 — Análise de logs de incidente (sem CoT)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Baseline técnico: análise direta de causa raiz a partir dos logs do payment-service sem CoT. Comparativo para o Experimento 04.

#### Prompt usado

```
Analise os logs abaixo de um incidente em produção e identifique a causa raiz.

[logs do payment-service — ver seção "Logs do incidente" neste arquivo]
```

#### Resposta da IA

Identificou "connection pool esgotado" como causa raiz, mas pulou direto para a conclusão sem mostrar o raciocínio temporal ou o cálculo de conexões totais.

#### Output completo da IA

**Causa Raiz:** Esgotamento do connection pool do PostgreSQL no payment-service.

Após o deploy da v2.3.1, os 3 pods inicializaram com `max_connections=20` cada (total: 60 conexões). O uso cresceu rapidamente até atingir o limite, causando timeouts de 5000ms e erros 500 no ALB. O RDS confirma 60/100 conexões ativas com CPU em 92%, indicando sobrecarga.

**Ação recomendada:** Aumentar `max_connections` ou implementar connection pooling externo (PgBouncer).

#### O que funcionou

- Identificou a causa raiz corretamente (connection pool esgotado)
- Correlacionou o deploy com o início do problema

#### O que não funcionou

- Sem reconstrução temporal da sequência de falha
- Não calculou explicitamente 3 pods × 20 = 60 conexões totais vs. limite de 100 do RDS
- Não explicou por que a carga cresceu repentinamente após o deploy
- Ausência de auditabilidade — impossível verificar o raciocínio

#### Ajustes que eu fiz

- Nenhum. Este prompt é o baseline intencional para o comparativo com CoT.

#### Nota (1 a 5)

**Nota:** 3/5

**Justificativa:** Resposta correta mas não auditável. Para uso em postmortem ou RCA formal, a ausência de raciocínio explícito compromete a credibilidade da análise.

#### O que eu faria diferente

Usar o Experimento 04 (CoT estruturado) para qualquer análise de incidente que precise de raciocínio auditável ou que alimente um postmortem formal.

---

### Experimento 04 — Análise de logs de incidente (com CoT estruturado)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Mesmo contexto do Experimento 03 com 5 etapas de raciocínio estruturado. Demonstra CoT encadeado — cada etapa constrói sobre a anterior para produzir análise auditável.

#### Prompt usado

```
Analise os logs abaixo de um incidente em produção e identifique a causa raiz.

Antes de concluir, raciocine passo a passo:
1. Identifique o evento inicial que desencadeou o incidente
2. Correlacione os timestamps para reconstruir a sequência de falha
3. Calcule os recursos envolvidos (conexões, réplicas, limites)
4. Explique por que o sistema falhou com base nos números
5. Apresente a causa raiz e o que a provocou

[logs do payment-service — ver seção "Logs do incidente" neste arquivo]
```

#### Resposta da IA

Análise em 5 etapas com reconstrução temporal, cálculo explícito de 3×20=60 conexões vs. limite de 100 do RDS, e causa raiz completa com narrativa auditável.

#### Output completo da IA

**Etapa 1 — Evento inicial:**
Deploy da v2.3.1 concluído às 14:30:15 com 3 réplicas. Esse é o gatilho.

**Etapa 2 — Sequência temporal:**
- 14:30 — Deploy concluído, 3 pods inicializados
- 14:31 — Todos os pools inicializados (max=20 cada)
- 14:32 — Sistema funcionando normalmente (200 OK, 23ms)
- 14:35–14:36 — Pools atingem 90-95% de utilização
- 14:37 — Pool esgotado → timeouts de 5s → erros 500
- 14:37:30 — Alarme CloudWatch (87% error rate)
- 14:40 — P1 aberto

**Etapa 3 — Cálculo de recursos:**
- 3 pods × max_connections=20 = 60 conexões totais para o RDS
- RDS confirma: 60/100 conexões ativas (60% do limite)
- RDS CPU: 92% — sobrecarga nas queries

**Etapa 4 — Por que falhou:**
O deploy criou 3 novos pods cada um com pool de 20 conexões. Dentro de 5 minutos, a demanda cresceu até esgotar o pool de cada pod individualmente. O RDS tem capacidade (60/100), mas o pool local de cada pod não aguenta o volume de requests concorrentes. O gargalo não é o RDS — é o `max_connections=20` por pod subdimensionado. A nova versão v2.3.1 provavelmente introduziu queries mais lentas (CPU do RDS em 92%), aumentando o tempo de ocupação de cada conexão e acelerando o esgotamento.

**Etapa 5 — Causa raiz:**
Connection pool subdimensionado (`max_connections=20`) por pod, insuficiente para o volume de requests concorrentes em produção. A v2.3.1 introduziu queries mais lentas (hipótese baseada no CPU do RDS em 92%), acelerando o esgotamento do pool.

#### O que funcionou

- Cálculo explícito e auditável de conexões (3×20=60)
- Distinção crucial: gargalo é o pool local, não o limite do RDS
- Hipótese sobre queries mais lentas na v2.3.1 emergiu do raciocínio — não estava no prompt
- Sequência temporal completa e verificável

#### O que não funcionou

- Não sugeriu ferramentas de investigação adicional (EXPLAIN ANALYZE, pg_stat_activity)
- Ações corretivas implícitas — o Experimento 05 resolve isso

#### Ajustes que eu fiz

- Nenhum ajuste necessário. Output alimentou diretamente o Experimento 05.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** CoT estruturado entregou análise auditável com raciocínio transparente e cálculo explícito. A hipótese sobre queries mais lentas foi emergente — surgiu do raciocínio encadeado, não do prompt.

#### O que eu faria diferente

Adicionar Etapa 6: "liste comandos de investigação imediata para validar a hipótese" para tornar a análise diretamente acionável durante o war room.

---

### Experimento 05 — Post-mortem encadeado (CoT em cadeia)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

CoT encadeado: usa o output do Experimento 04 como contexto para gerar um post-mortem estruturado. Demonstra como o raciocínio de uma etapa alimenta o próximo artefato sem retrabalho.

#### Prompt usado

```
Com base na análise de causa raiz acima, gere um documento de post-mortem estruturado. Raciocine passo a passo para:

1. Definir a severidade e o impacto com base nos dados dos logs
2. Montar a timeline com os marcos críticos
3. Derivar ações corretivas a partir da causa raiz identificada
4. Sugerir ações preventivas para evitar recorrência

Use o formato:
- Título do incidente
- Data, duração, severidade
- Resumo executivo
- Timeline
- Causa raiz
- Ações corretivas (com responsável sugerido e prazo)
- Ações preventivas
```

#### Resposta da IA

Post-mortem completo com todos os campos preenchidos a partir da análise do Experimento 04, incluindo ações corretivas com responsável e prazo derivadas da causa raiz identificada.

#### Output completo da IA

**Título:** Incidente P1 — payment-service indisponível após deploy v2.3.1

**Data:** 2025-11-15 | **Duração:** ~10 minutos (14:30–14:40) | **Severidade:** P1 / Critical

**Resumo Executivo:**
Após o deploy da v2.3.1 do payment-service, o connection pool de cada pod (max=20) foi esgotado em ~7 minutos devido ao volume de requests concorrentes. A nova versão introduziu queries mais lentas (RDS CPU 92%), aumentando o tempo de ocupação de cada conexão. Com 3 pods × 20 conexões = 60 totais, o pool individual tornou-se o gargalo. Error rate atingiu 87% antes da abertura do P1.

**Timeline:**
- 14:30:01 — Deploy v2.3.1 iniciado
- 14:30:15 — 3 réplicas atualizadas com sucesso
- 14:32:00 — Sistema operando normalmente (200 OK, p99 23ms)
- 14:35–14:36 — Pools atingem 90-95% de utilização
- 14:37:01 — Primeiro timeout de pool (5000ms)
- 14:37:02 — Primeiros erros 500 no ALB
- 14:37:30 — Alarme CloudWatch: error rate 87%
- 14:40:00 — P1 aberto via PagerDuty

**Causa Raiz:**
Connection pool subdimensionado (max_connections=20/pod) combinado com aumento de latência de queries na v2.3.1, acelerando o esgotamento do pool sob carga de produção.

**Ações Corretivas:**

| Ação | Responsável | Prazo |
|------|-------------|-------|
| Aumentar max_connections para 50/pod e monitorar | SRE on-call | Imediato |
| Implementar PgBouncer para pooling externo | Infra team | 1 semana |
| Fazer EXPLAIN ANALYZE nas queries novas da v2.3.1 | Dev team | 48h |

**Ações Preventivas:**
- Adicionar teste de carga em staging antes de deploys no payment-service
- Criar alerta de pool usage > 70% (atualmente só há alerta de error rate)
- Definir `max_connections` como parâmetro obrigatório revisado em code review
- Adicionar connection pool metrics ao dashboard de deploy (canary)

#### O que funcionou

- Post-mortem completo gerado sem retrabalho — tudo derivado do Experimento 04
- Ações corretivas com responsável e prazo específicos
- Timeline precisa construída a partir dos timestamps dos logs
- Ações preventivas endereçam tanto o sintoma quanto a causa raiz

#### O que não funcionou

- Impacto ao negócio não quantificado — sem dados de transações perdidas ou SLA
- Não incluiu seção "o que funcionou bem" (resposta rápida do on-call, alarme CloudWatch eficaz)

#### Ajustes que eu fiz

- Em uso real: adicionar ao prompt "inclua impacto estimado em transações e receita" e "adicione seção 'o que funcionou bem' para reconhecer a resposta eficaz do time".

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** CoT encadeado eliminou a necessidade de reescrever contexto entre análise e postmortem. O output do Experimento 04 alimentou diretamente este com fidelidade total aos dados dos logs. Padrão aplicável em qualquer pipeline de resposta a incidentes.

#### O que eu faria diferente

Documentar o pipeline completo como template de war room: Experimento 03 (análise rápida) → Experimento 04 (análise detalhada) → Experimento 05 (postmortem) com contexto passado explicitamente entre etapas.
