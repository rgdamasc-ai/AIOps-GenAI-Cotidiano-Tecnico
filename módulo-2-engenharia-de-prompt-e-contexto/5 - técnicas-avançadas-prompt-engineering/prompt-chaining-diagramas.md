# Prompt Chaining - Diagrama

Os prompts abaixo formam duas cadeias independentes. Cada prompt depende do output do anterior — copie e execute em sequência na mesma sessão de chat. Os placeholders `[colar output do Prompt N]` indicam onde inserir a resposta anterior.

---

## Cadeia 1 — Post para LinkedIn em duas etapas

### Passo 1 — Rascunho de conteúdo

Foco exclusivo na mensagem e no exemplo concreto. Formatação e engajamento são deliberadamente adiados para o próximo passo.

```
Sou um profissional de DevOps e quero escrever um post para LinkedIn
sobre uma lição importante que aprendi: automatizar sem entender o
processo manual primeiro leva a automações frágeis que quebram em
cenários não previstos.

Escreva um rascunho do post focando apenas no conteúdo e na mensagem.
Não se preocupe com hook, tom ou call-to-action por enquanto.
Inclua um exemplo concreto que ilustre a lição.
```

### Passo 2 — Refinamento para engajamento

Recebe o rascunho do Passo 1 e aplica as regras de formatação de LinkedIn: hook, parágrafos curtos e pergunta final. Execute na mesma janela de contexto (o rascunho já está no histórico).

```
Pegue o rascunho abaixo e reescreva como um post de LinkedIn otimizado
para engajamento:

- Comece com um hook de 1-2 linhas que gere curiosidade
- Use tom conversacional e direto (como se estivesse falando com um
  colega)
- Quebre em parágrafos curtos (máximo 3 linhas cada)
- Termine com uma pergunta que convide comentários

Rascunho:
[colar o output do Prompt 1]
```

---

## Cadeia 2 — Pipeline de incidente em 4 etapas

Simula um fluxo completo de resposta a incidente: triagem → diagnóstico → remediação → documentação. Cada prompt recebe o output do anterior como entrada. Os dados brutos do incidente estão no Passo 1.

### Passo 1 — Triagem dos dados

Organiza os dados brutos sem diagnosticar. O objetivo é estruturar fatos, padrões e correlações antes de qualquer hipótese.

```
Você é um SRE fazendo triagem de um incidente em produção.
Abaixo estão os dados brutos coletados do ambiente.

Sua tarefa: organizar esses dados num relatório de triagem
estruturado. Identifique padrões, correlações temporais e anomalias.
NÃO tente diagnosticar ainda. Apenas organize os fatos.

LOGS DA APLICAÇÃO (últimas 2h, filtrado por ERROR):
14:02:31 ERROR [req-a8f1] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:02:31 ERROR [req-a8f1] Handler /api/orders: 500 Internal Server Error
14:03:15 INFO  [req-b2c9] Handler /api/health: 200 OK
14:03:47 ERROR [req-d4e7] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:03:47 ERROR [req-d4e7] Handler /api/users: 500 Internal Server Error
14:04:02 INFO  [req-f1a3] Handler /api/orders: 200 OK (432ms)
14:04:55 ERROR [req-c7d2] DBPool: Pool exhausted, waiting for available connection... timeout after 5000ms
14:04:55 ERROR [req-c7d2] Handler /api/products: 500 Internal Server Error
14:05:11 INFO  [req-e9b4] Handler /api/users: 200 OK (387ms)
14:06:03 ERROR [req-h3j8] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:08:22 ERROR [req-k5m1] DBPool: Pool exhausted, waiting for available connection... timeout after 5000ms

MÉTRICAS DO RDS (CloudWatch, últimas 2h):
- DatabaseConnections: 98-100 (limite: max_connections=100)
- CPUUtilization: 92-96%
- FreeableMemory: 1.2GB de 8GB
- ReadLatency: 45ms (normal: 5-8ms)
- WriteLatency: 62ms (normal: 8-12ms)

STATUS DOS PODS (kubectl):
NAME                        READY   STATUS    RESTARTS   AGE
api-server-6d4f8b7c9-abc12  1/1     Running   0          18h
api-server-6d4f8b7c9-def34  1/1     Running   0          18h
api-server-6d4f8b7c9-ghi56  1/1     Running   0          18h

TRECHO DO ÚLTIMO DEPLOY (diff do values.yaml, ontem 18h):
  db:
    pool:
-     size: 10
+     size: 50
    host: prod-db.abc123.us-east-1.rds.amazonaws.com

AMBIENTE:
- App: Node.js, EKS com 3 réplicas
- Banco: PostgreSQL RDS (db.r5.large, max_connections=100)
- Início dos erros: 14h hoje. Erros afetam ~30% das requisições.

Gere o relatório de triagem no formato:
1. Resumo da situação (3 linhas)
2. Padrões identificados nos logs (com evidências)
3. Anomalias nas métricas (comparando com valores normais)
4. Mudanças recentes relevantes
5. Correlações entre os dados
```

### Passo 2 — Diagnóstico da causa raiz

Recebe o relatório de triagem e constrói a cadeia causal completa com evidências e hipóteses descartadas.

```
Com base no relatório de triagem abaixo, diagnostique a causa raiz
deste incidente.

[colar output do Prompt 1]

Apresente:
1. Causa raiz principal (com a cadeia causal completa: o que mudou →
   o que isso causou → por que gerou o sintoma observado)
2. Evidências dos dados que sustentam o diagnóstico
3. Hipóteses alternativas consideradas e por que foram descartadas
```

### Passo 3 — Plano de remediação

Recebe o diagnóstico e propõe ações em duas fases: mitigação imediata e correção definitiva, ambas com comandos específicos e critérios de validação.

```
Com base no diagnóstico abaixo, proponha um plano de remediação.

[colar output do Prompt 2]

Contexto técnico:
- App: Node.js em EKS, 3 réplicas
- Banco: PostgreSQL RDS db.r5.large, max_connections=100
- Pool size atual: 50 por réplica (configurado em values.yaml)
- Pool size anterior: 10 por réplica

Proponha um plano em duas fases:
1. Ação imediata para parar o sangramento (mitigação)
2. Correção definitiva para evitar recorrência

Para cada ação, indique: o que fazer, comando ou configuração
específica, risco envolvido e como validar que funcionou.
```

### Passo 4 — Post-mortem

Consolida triagem, diagnóstico e remediação em um documento formal de post-mortem pronto para compartilhar com a equipe.

```
Com base nas informações abaixo, gere a documentação post-mortem
deste incidente.

Relatório de triagem:
[output do Prompt 1]

Diagnóstico:
[output do Prompt 2]

Remediação aplicada:
[output do Prompt 3]

Gere o post-mortem no formato:
- Resumo do incidente (3 linhas)
- Timeline (desde o deploy até a resolução)
- Causa raiz
- Impacto (usuários afetados, duração, endpoints impactados)
- Ações tomadas (imediatas e definitivas)
- Action items para prevenir recorrência
```

---

## Caderno de Experimentos

---

### Experimento 01 — Prompt Chaining: Post LinkedIn em duas etapas

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

#### Contexto

Demonstração da técnica Prompt Chaining com dois passos sequenciais para criação de conteúdo: primeiro o conteúdo bruto (mensagem e exemplo concreto), depois a otimização para engajamento no LinkedIn. A separação de responsabilidades entre passos é o diferencial da técnica.

#### Prompt usado

**Passo 1 — Rascunho de conteúdo:**

```
Sou um profissional de DevOps e quero escrever um post para LinkedIn
sobre uma lição importante que aprendi: automatizar sem entender o
processo manual primeiro leva a automações frágeis que quebram em
cenários não previstos.

Escreva um rascunho do post focando apenas no conteúdo e na mensagem.
Não se preocupe com hook, tom ou call-to-action por enquanto.
Inclua um exemplo concreto que ilustre a lição.
```

**Passo 2 — Refinamento para engajamento:**

```
Pegue o rascunho abaixo e reescreva como um post de LinkedIn otimizado
para engajamento:
- Comece com um hook de 1-2 linhas que gere curiosidade
- Use tom conversacional e direto
- Quebre em parágrafos curtos (máximo 3 linhas cada)
- Termine com uma pergunta que convide comentários

Rascunho:
[output do Passo 1]
```

#### Resposta da IA

**Passo 1:** Rascunho com exemplo concreto de deploy automatizado que derrubou produção. Focou exclusivamente na mensagem sem tentar otimizar o formato — exatamente o que foi pedido.

**Passo 2:** Transformou o rascunho com hook impactante, parágrafos curtos e pergunta de encerramento. O passo 2 não precisou de nenhuma instrução adicional além do rascunho e das regras de formatação.

#### Output completo da IA

**Output do Passo 1:**

A lição que aprendi da forma mais difícil: automatizar sem entender o processo manual leva a desastres silenciosos.

Quando entrei no time de DevOps, havia um processo manual de deploy que levava 45 minutos. Eram 12 etapas documentadas num Confluence antigo, cheias de notas como "só rodar se X estiver saudável" e "aguardar 3 minutos antes do passo seguinte porque o serviço Y é lento". O time odiava esse processo. Eu decidi automatizá-lo.

Em duas semanas, criei um pipeline que executava todas as etapas automaticamente. Os deploys caíram para 8 minutos. Todos comemoraram.

Três meses depois, num deploy de sexta à noite, o pipeline falhou de forma que ninguém havia previsto. O serviço Y estava lento por pico de carga — algo que acontecia raramente, mas acontecia. O pipeline não esperou. Ele sobrescreveu uma configuração que o serviço Y ainda não havia lido, e derrubamos produção por 2 horas.

O problema não era o pipeline. O problema era que eu havia automatizado o "o quê" sem entender o "por quê". Cada nota estranha no Confluence existia por uma razão — razões que só surgem depois de anos de incidentes que eu nunca vivi.

A automação transformou um processo com guardrails humanos em um processo sem guardrails — e muito mais rápido para falhar.

---

**Output do Passo 2:**

Automatizei um deploy e derrubei produção. A culpa não foi da automação.

Quando entrei no time de DevOps, havia um processo manual de 45 minutos com 12 etapas. Tinha notas esquisitas no Confluence: "aguarde 3 min antes do passo seguinte" e "só rodar se X estiver saudável".

Automatizei tudo em duas semanas. De 45 min para 8 min. Todo mundo comemorou.

Três meses depois, numa sexta à noite, o pipeline falhou de um jeito que ninguém previu. O serviço Y estava lento por pico de carga. O pipeline não esperou. Sobrescrevemos uma configuração que o serviço ainda não havia lido. Duas horas fora do ar.

O problema não era o pipeline. Era eu.

Automatizei o "o quê" sem entender o "por quê". Cada nota esquisita no Confluence havia sobrevivido a um incidente que eu nunca vivi. Removi todos os guardrails humanos e fiz o processo falhar mais rápido.

Aprendi que automação sem compreensão é só incompetência em alta velocidade.

Você já automatizou algo que quebrou de um jeito que você não previu? Como você abordaria diferente hoje?

#### O que funcionou

- A separação de responsabilidades funcionou: o Passo 1 não tentou otimizar o formato, mantendo foco na substância
- O Passo 2 aplicou as regras sem precisar de contexto extra — o rascunho era suficientemente rico
- A frase "automação sem compreensão é só incompetência em alta velocidade" emergiu espontaneamente no Passo 2

#### O que não funcionou

- Sem falhas significativas nesta cadeia curta
- O hook poderia ser mais forte se "2 horas fora do ar" aparecesse logo na primeira linha

#### Ajustes que eu fiz

- Nenhum ajuste necessário. Dois passos foram suficientes para produzir output de qualidade.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O chaining funcionou exatamente como proposto. Separar "gerar conteúdo" de "otimizar formato" evitou trade-offs ruins entre substância e engajamento.

#### O que eu faria diferente

Adicionaria um Passo 3 de revisão de tom: verificar se o post não soa arrogante ou autopromocional — risco real em posts de "lição aprendida" no LinkedIn.

---

### Experimento 02 — Prompt Chaining: Pipeline de Incidente em 4 Etapas

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Troubleshooting

#### Contexto

Cadeia de 4 prompts sequenciais simulando resposta completa a incidente: triagem → diagnóstico → remediação → post-mortem. Dados brutos de um incidente de saturação de conexões em RDS PostgreSQL foram fornecidos no Passo 1. Cada prompt recebe o output do anterior como entrada.

#### Prompt usado

**Passo 1 — Triagem:** Prompt completo com logs de aplicação, métricas CloudWatch, status de pods kubectl e diff do deploy anterior (conforme arquivo fonte).

**Passo 2 — Diagnóstico:**
```
Com base no relatório de triagem abaixo, diagnostique a causa raiz
deste incidente.
[output do Passo 1]

Apresente: causa raiz com cadeia causal completa, evidências e
hipóteses alternativas descartadas.
```

**Passo 3 — Remediação:**
```
Com base no diagnóstico abaixo, proponha um plano de remediação.
[output do Passo 2]
Plano em duas fases: mitigação imediata e correção definitiva,
com comandos específicos e critérios de validação.
```

**Passo 4 — Post-mortem:**
```
Gere a documentação post-mortem com base em:
Triagem: [output Passo 1]
Diagnóstico: [output Passo 2]
Remediação: [output Passo 3]
```

#### Resposta da IA

**Passo 1:** Relatório estruturado em 5 seções. A IA identificou o diff do values.yaml (`pool.size: 10→50`) como mudança relevante e calculou nas correlações que 3 × 50 = 150 > 100 — sem diagnosticar ainda.

**Passo 2:** Diagnóstico preciso com cadeia causal completa e 4 hipóteses alternativas descartadas com evidências.

**Passo 3:** Plano em duas fases com comandos kubectl/helm executáveis, fórmula matemática para pool size correto e critérios de validação.

**Passo 4:** Post-mortem com timeline, causa raiz, impacto e action items com responsáveis e prazos.

#### Output completo da IA

**Output do Passo 1 (Triagem):**

## Relatório de Triagem

### 1. Resumo da situação
A partir das 14h02, ~30% das requisições retornam erro 500 com falhas de conexão ao PostgreSQL (10.0.3.42:5432). O banco opera no limite de conexões (98-100/100), com CPU elevada (92-96%) e latências 5-10x acima do normal. Os pods EKS estão saudáveis e sem reinícios.

### 2. Padrões identificados nos logs

**Padrão 1 — ECONNREFUSED recorrente:**
- 14:02:31, 14:03:47, 14:06:03: `DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432`
- Afeta múltiplos endpoints (/api/orders, /api/users) — não é falha de endpoint específico

**Padrão 2 — Pool exhausted:**
- 14:04:55 e 14:08:22: `DBPool: Pool exhausted, waiting for available connection... timeout after 5000ms`

**Padrão 3 — Intermitência:**
- Requests bem-sucedidos têm latência 387-432ms (normal: <10ms)
- /api/health retorna 200 consistentemente (não usa DB)

### 3. Anomalias nas métricas

| Métrica | Normal | Atual | Desvio |
|---|---|---|---|
| DatabaseConnections | <70 (estimado) | 98-100/100 | No limite máximo |
| ReadLatency | 5-8ms | 45ms | +562% |
| WriteLatency | 8-12ms | 62ms | +516% |
| CPUUtilization | — | 92-96% | Crítico |

### 4. Mudanças recentes relevantes

Deploy de ontem 18h — diff values.yaml:
```yaml
db:
  pool:
-   size: 10
+   size: 50
```
Com 3 réplicas: 3 × 50 = **150 conexões tentadas** vs. max_connections=100.

### 5. Correlações

1. Deploy elevou total de 30 para 150 conexões — excede o limite em 50%
2. Banco satura (98-100/100) → rejeita conexões excedentes (ECONNREFUSED)
3. CPU alta (92-96%) é efeito da contenção de conexões, não causa
4. Latências elevadas são sintoma do banco degradado por saturação
5. Gap de 20h deploy→erros: carga gradual preencheu o pool progressivamente

---

**Output do Passo 2 (Diagnóstico):**

## Diagnóstico da Causa Raiz

### 1. Causa raiz

**Misconfiguration de pool size sem considerar max_connections e número de réplicas.**

**Cadeia causal:**
1. Deploy (ontem 18h): `pool.size: 10 → 50` por réplica
2. 3 réplicas × 50 = 150 conexões tentadas vs. limite de 100 no RDS
3. Banco rejeita conexões excedentes → ECONNREFUSED
4. Conexões que passam enfrentam contenção → CPU 92-96%, latências 5-10x acima do normal
5. Pool da aplicação esgota aguardando conexões → timeout 5000ms → Pool exhausted
6. ~30% das requisições retornam 500 Internal Server Error

### 2. Evidências

- **ECONNREFUSED** (não timeout): banco recusa ativamente, não é falha de rede
- **DatabaseConnections 98-100/100**: saturação sustentada no limite máximo
- **Pool exhausted**: aplicação não consegue mais alocar conexões
- **CPU 92-96%**: PostgreSQL sob pressão de lock contention e gerenciamento de conexões
- **ReadLatency +562%**: padrão típico de banco saturado por conexões, não por volume de queries
- **0 restarts nos pods**: descarta problema na camada de aplicação

### 3. Hipóteses descartadas

| Hipótese | Por que descartada |
|---|---|
| Falha de rede | ECONNREFUSED = recusa ativa. Conexões que passam funcionam. |
| Bug no código | Nenhuma mudança de código. Apenas values.yaml. |
| Queries pesadas | CPU alta é efeito da contenção. FreeableMemory adequada (1.2GB). |
| Hardware RDS | Memória não é gargalo. Padrão é de saturação de conexões. |

---

**Output do Passo 3 (Remediação):**

## Plano de Remediação

### Fase 1 — Mitigação imediata (objetivo: <5 min)

**Reverter pool size para 10 por réplica:**

```bash
# Editar values.yaml: pool.size: 50 → 10
helm upgrade api-server ./chart -f values.yaml -n production --wait

# Validação
watch -n5 "psql -h prod-db.abc123.us-east-1.rds.amazonaws.com \
  -c 'SELECT count(*) FROM pg_stat_activity;'"
kubectl logs -l app=api-server -n production --since=1m | grep -c ERROR
```

**Risco:** Rolling update causa reinício gradual dos pods (~30-60s por pod). Impacto menor que 30% de falha contínua.

### Fase 2 — Correção definitiva

**Cálculo correto do pool size:**
```
max_connections         = 100
Reservado (admin/sys)   = 10
Disponível para app     = 90
Réplicas                = 3
Pool por réplica        = 90 / 3 = 30
Com margem (75%)        = 22 → arredondado: 20

Permite escalar para 4 réplicas: 4 × 20 = 80 < 90 ✓
```

```yaml
# values.yaml
db:
  pool:
    size: 20
    minSize: 5
    acquireTimeout: 3000
```

**Ações adicionais:**
1. Alerta CloudWatch: `DatabaseConnections > 80`
2. Documentar no runbook: `pool_size = floor((max_connections - 10) / replicas * 0.75)`
3. Validação no CI/CD: bloquear deploy se `pool_size × replicas > max_connections × 0.85`

---

**Output do Passo 4 (Post-mortem):**

## Post-Mortem — Saturação de Conexões no RDS (SEV-2)

### Resumo
Alteração de pool de banco de 10 para 50 conexões por réplica elevou o total de 30 para 150, ultrapassando o limite de 100 do RDS. O banco saturou progressivamente em ~20h, causando 30% de falha nas requisições. Resolvido com reversão do pool size.

### Timeline

| Horário | Evento |
|---|---|
| Ontem 18h00 | Deploy: `pool.size: 10 → 50` em values.yaml |
| Ontem 18h – hoje 14h | Carga gradual consome o pool progressivamente |
| Hoje 14h02 | Primeiros ECONNREFUSED nos logs |
| Hoje 14h04 | Pool exhausted timeout registrado |
| Hoje 14h08 | Alertas acionados; investigação iniciada |
| Hoje 14h+X | Causa raiz identificada; remediação aplicada |
| Hoje 14h+X+5 | DatabaseConnections caindo; erros cessando |

### Causa raiz
Miscalibração de pool size sem considerar `max_connections` e número de réplicas.
Fórmula usada (errada): `pool_size = 50` sem contexto.
Fórmula correta: `floor((max_connections - reservado) / replicas * 0.75)`.

### Impacto
- **Usuários afetados:** ~30% das requisições com erro 500
- **Endpoints:** /api/orders, /api/users, /api/products
- **Duração:** ~1h de degradação severa
- **Ambiente:** Produção (EKS 3 réplicas + RDS db.r5.large)

### Ações tomadas
**Imediatas:** `pool.size: 50 → 10` via helm upgrade com rolling update.
**Definitivas:** Pool recalibrado para 20. Alerta CloudWatch criado. Runbook atualizado.

### Action items

| Item | Responsável | Prazo |
|---|---|---|
| Validação CI/CD: bloquear se `pool × réplicas > max_connections × 0.85` | DevOps | 1 semana |
| Documentar fórmula de pool no runbook | SRE | Esta semana |
| Alerta CloudWatch: DatabaseConnections > 80 | SRE | Hoje |
| Revisar pool sizes em staging | DevOps | 2 semanas |
| Avaliar upgrade RDS para db.r5.xlarge | Arquitetura | Próximo quarter |

#### O que funcionou

- A instrução "NÃO tente diagnosticar ainda" no Passo 1 manteve o foco na triagem e evitou hipóteses prematuras
- O modelo calculou corretamente 3 × 50 = 150 > 100 usando apenas os dados fornecidos
- O post-mortem emergiu coeso porque os 3 passos anteriores já estavam estruturados

#### O que não funcionou

- O Passo 4 precisa dos 3 outputs colados manualmente — em conversas longas há risco de perda de contexto
- O modelo não sinalizou FreeableMemory (15% disponível) como preocupação secundária

#### Ajustes que eu fiz

- Adicionei contexto técnico explícito no Passo 3 (versões e configurações atuais) que não estava no diagnóstico, necessário para gerar comandos específicos

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** A cadeia de 4 passos produziu diagnóstico preciso, remediação com fórmulas reutilizáveis e post-mortem pronto para compartilhar. Cada passo cumpriu exatamente seu papel.

#### O que eu faria diferente

Adicionaria um Passo 0 antes da triagem com contexto de negócio (SLA, número de usuários afetados, severidade definida pelo on-call) para que o post-mortem saísse com impacto de negócio calculado, não apenas impacto técnico.
