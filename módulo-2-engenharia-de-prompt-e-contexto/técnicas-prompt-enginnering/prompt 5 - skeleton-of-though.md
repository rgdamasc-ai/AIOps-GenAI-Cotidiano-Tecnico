# Skeleton-of-Thought Prompting

---

## Prompt 1 — Guia de mudança de carreira para TI

Prompt da demo não-técnica (Passo 1). Pede ao modelo que primeiro liste o esqueleto do guia e só depois expanda cada tópico. Mostra a previsibilidade do SoT: você valida a estrutura antes de receber o conteúdo final.

```
Vou criar um guia completo para quem quer mudar de carreira e entrar na área de TI como DevOps/Cloud Engineer.

Primeiro, liste o esqueleto do guia — os principais tópicos que serão cobertos, sem detalhar ainda.
Depois, expanda cada tópico com conteúdo detalhado, exemplos práticos e recursos.
Retorne em markdown.
```

---

## Prompt 2 — Análise estruturada de log de incidente

Prompt da Demo Técnica (Passo 2). O modelo gera o esqueleto da análise (Resumo Executivo, Timeline, Causa Raiz, Impacto, Evidências, Ações) antes de preencher com dados dos logs. Resultado: análise com estrutura de post-mortem profissional.

```
Analise os logs de incidente abaixo e produza uma análise estruturada do ocorrido.

Primeiro, crie o esqueleto da análise — as seções que serão cobertas (sem detalhar ainda).
Depois, expanda cada seção com base nos logs.

Logs:
[ver arquivo "Logs do incidente" abaixo]
```

---

## Prompt 3 — Post-mortem com esqueleto fornecido pelo usuário

Prompt da Demo Técnica (Passo 3). Demonstra a **segunda modalidade do SoT**: você fornece a estrutura e o modelo só preenche o conteúdo. Útil quando a empresa já tem template formal de post-mortem a ser seguido.

```
Com base na análise de incidente gerada acima, crie um documento formal de post-mortem seguindo este esqueleto:

1. Título e Metadados (data, severidade, responsáveis)
2. Resumo Executivo (máximo 3 parágrafos)
3. Timeline Detalhada
4. Análise de Causa Raiz (5 Whys)
5. Impacto ao Negócio
6. Ações Imediatas Tomadas
7. Ações Preventivas
8. Lições Aprendidas

Expanda cada seção sem implementar ainda.
Depois, expanda os tópicos e sub tópicos gerando o documento para compartilhar com a equipe de engenharia.
```

---

## Logs do incidente (database connection timeout)

Bloco de logs fictícios usados no Prompt 2. Reaproveite o output dele como contexto do Prompt 3.

```
[2024-03-15 02:14:33] ERROR app-server-01: Database connection timeout after 30s (attempt 1/3)
[2024-03-15 02:14:35] ERROR app-server-02: Database connection timeout after 30s (attempt 1/3)
[2024-03-15 02:14:38] ERROR app-server-01: Database connection timeout after 30s (attempt 2/3)
[2024-03-15 02:14:40] WARN  load-balancer: Upstream app-server-01 health check failed
[2024-03-15 02:14:41] ERROR app-server-02: Database connection timeout after 30s (attempt 2/3)
[2024-03-15 02:14:43] CRITICAL app-server-01: All retry attempts exhausted. Service degraded
[2024-03-15 02:14:45] CRITICAL app-server-02: All retry attempts exhausted. Service degraded
[2024-03-15 02:14:46] WARN  load-balancer: Upstream app-server-02 health check failed
[2024-03-15 02:14:50] ERROR db-primary: Max connections reached (current: 500/500)
[2024-03-15 02:14:51] CRITICAL load-balancer: All upstreams unhealthy. Returning 503 to clients
[2024-03-15 02:15:10] INFO  on-call-alert: PagerDuty alert triggered — P1 incident opened
[2024-03-15 02:31:22] INFO  db-primary: Connections dropped to 45/500 after connection pool restart
[2024-03-15 02:31:25] INFO  app-server-01: Database connection established
[2024-03-15 02:31:26] INFO  app-server-02: Database connection established
[2024-03-15 02:31:28] INFO  load-balancer: All upstreams healthy
```

---

## Caderno de Experimentos

---

### Experimento 01 — Guia de mudança de carreira para TI

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Teste de SoT para criação de guia completo de transição de carreira para DevOps/Cloud. O modelo gera primeiro o esqueleto (tópicos) e só depois expande, permitindo validação da estrutura antes do conteúdo.

#### Prompt usado

```
Vou criar um guia completo para quem quer mudar de carreira e entrar na área de TI como DevOps/Cloud Engineer.

Primeiro, liste o esqueleto do guia — os principais tópicos que serão cobertos, sem detalhar ainda.
Depois, expanda cada tópico com conteúdo detalhado, exemplos práticos e recursos.
Retorne em markdown.
```

#### Resposta da IA

Esqueleto gerado primeiro com 7 tópicos claros, depois expandido com conteúdo detalhado, exemplos e recursos por seção — sem misturar as duas fases.

#### Output completo da IA

**Fase 1 — Esqueleto:**
1. Por que DevOps/Cloud é uma boa transição
2. O que você precisa saber antes de começar
3. Roadmap de habilidades técnicas essenciais
4. Certificações que valem o investimento
5. Como construir seu portfólio do zero
6. Como conseguir a primeira oportunidade
7. O que esperar nos primeiros 6 meses

**Fase 2 — Expansão (resumo por tópico):**

**1. Por que DevOps/Cloud é uma boa transição:**
Mercado aquecido, salários competitivos, trabalho remoto comum. Empresas de todos os setores precisam de profissionais de infraestrutura moderna. A transição é viável mesmo sem experiência prévia em TI — lógica e resolução de problemas transferem.

**2. O que você precisa saber antes de começar:**
Expectativa de 6-12 meses até a primeira contratação. Investimento em cursos e certificações (~R$2.000-5.000). Inglês técnico é diferencial real para leitura de documentação.

**3. Roadmap de habilidades:**
Linux (linha de comando, permissões, processos) → Redes básicas (TCP/IP, DNS, HTTP) → Git → Docker → Cloud provider (AWS ou Azure) → Kubernetes (após dominar Docker) → Terraform → CI/CD.

**4. Certificações:**
AWS Cloud Practitioner (entrada, ~$100) → AWS Solutions Architect Associate (mercado, ~$150) → CKA para Kubernetes (avançado, ~$395). Priorizar prática antes da prova.

**5. Portfólio do zero:**
Criar conta gratuita na AWS/GCP, subir aplicação em container, usar Terraform para provisionar, colocar no GitHub com README detalhado. Projetos reais valem mais que projetos de curso.

**6. Primeira oportunidade:**
Júnior DevOps, SRE trainee, suporte N2 em cloud. Empresas de consultoria são boa porta de entrada. LinkedIn e comunidades (Telegram, Discord) são canais efetivos no Brasil.

**7. Primeiros 6 meses:**
Curva de aprendizado intensa. Foco em resolver problemas reais do time antes de propor mudanças. Runbooks e documentação são suas melhores ferramentas de integração.

#### O que funcionou

- Esqueleto validável antes do conteúdo — se um tópico estivesse errado, seria corrigido sem reescrever tudo
- Estrutura lógica de progressão (por quê → o quê → como → o que esperar)
- Exemplos práticos em cada seção sem precisar ser solicitados

#### O que não funcionou

- Tópico 2 poderia ser integrado ao Tópico 1 — informação de pré-requisitos é parte do "por que"
- Recursos (links, cursos) não foram listados — o prompt pediu "recursos" mas não foram incluídos

#### Ajustes que eu fiz

- Em uso real: adicionar "inclua pelo menos 2 recursos (links ou nomes de cursos) por tópico" para garantir que recursos sejam incluídos.

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** SoT funcionou perfeitamente — esqueleto gerado, validável, depois expandido. Desconto 1 ponto porque "recursos" não foram incluídos apesar de estar no prompt, exigindo iteração adicional.

#### O que eu faria diferente

Especificar no prompt: "na fase do esqueleto, inclua também uma linha descrevendo o objetivo de cada tópico". Isso tornaria a validação do esqueleto ainda mais informada antes de solicitar a expansão.

---

### Experimento 02 — Análise estruturada de log de incidente

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de SoT técnico: o modelo gera primeiro o esqueleto da análise (seções) antes de preencher com dados dos logs de incidente (database connection timeout). Demonstra previsibilidade de estrutura antes do conteúdo.

#### Prompt usado

```
Analise os logs de incidente abaixo e produza uma análise estruturada do ocorrido.

Primeiro, crie o esqueleto da análise — as seções que serão cobertas (sem detalhar ainda).
Depois, expanda cada seção com base nos logs.

Logs:
[ver seção "Logs do incidente" neste arquivo]
```

#### Resposta da IA

Esqueleto com 6 seções de post-mortem gerado primeiro; depois expandido com dados extraídos diretamente dos logs, produzindo análise com estrutura profissional.

#### Output completo da IA

**Fase 1 — Esqueleto:**
1. Resumo Executivo
2. Timeline do Incidente
3. Causa Raiz
4. Impacto
5. Evidências nos Logs
6. Ações Imediatas e Próximos Passos

**Fase 2 — Expansão:**

**1. Resumo Executivo:**
Incidente de indisponibilidade total dos app-servers em 2024-03-15 às 02:14 causado por esgotamento de conexões no banco de dados primário. Duração: ~17 minutos (02:14 a 02:31). Resolução: reinício do connection pool no db-primary.

**2. Timeline:**
- 02:14:33 — Primeiros timeouts de conexão (30s) nos app-servers (tentativa 1/3)
- 02:14:40-46 — Health checks falham para app-server-01 e app-server-02
- 02:14:43-45 — Ambos os app-servers em estado "degraded" após esgotar retries
- 02:14:50 — db-primary: max connections atingido (500/500)
- 02:14:51 — Load balancer retornando 503 para todos os clientes
- 02:15:10 — PagerDuty: P1 aberto
- 02:31:22 — db-primary: conexões liberadas após reinício do pool (45/500)
- 02:31:25-28 — App-servers e load balancer restaurados

**3. Causa Raiz:**
Esgotamento de conexões no db-primary (500/500). Causa provável: connection leak ou spike de conexões sem retry adequado, levando ao limite máximo configurado no PostgreSQL.

**4. Impacto:**
~17 minutos de indisponibilidade total. Todos os clientes receberam 503. Período de baixo tráfego (02h) minimizou impacto, mas dependendo do SLA pode caracterizar violação.

**5. Evidências:**
- `Max connections reached (current: 500/500)` — confirmação do gargalo
- Tentativas 1/3 → 2/3 → "All retry attempts exhausted" — aplicação com retry mas sem circuit breaker
- `Connections dropped to 45/500 after connection pool restart` — solução foi reinício do pool

**6. Ações:**
- Imediato: aumentar `max_connections` ou implementar connection pooling (PgBouncer)
- Próximos passos: adicionar alerta de connections > 80%, investigar causa do spike, implementar circuit breaker

#### O que funcionou

- Esqueleto validável antes do conteúdo — estrutura de post-mortem profissional gerada automaticamente
- Dados dos logs extraídos com precisão (timestamps, valores exatos de conexões)
- Hipótese de causa raiz (connection leak) emergiu da análise, não estava nos logs explicitamente

#### O que não funcionou

- Seção de "Impacto" sem dados de SLA — precisaria de contexto adicional para quantificar a violação
- "Evidências" poderia ser mais granular (citar linha exata do log, não parafrasear)

#### Ajustes que eu fiz

- Nenhum ajuste necessário para esta demo. Em uso real, fornecer o SLA do serviço no prompt para quantificar impacto automaticamente.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** SoT entregou estrutura de post-mortem profissional a partir dos logs, sem template pré-definido. A fase de esqueleto torna o processo validável e reproduz o mesmo padrão estrutural independente do incidente.

#### O que eu faria diferente

Usar este experimento como fase 1 e o Experimento 03 como fase 2: gerar o esqueleto automaticamente (SoT), validar, depois usar o esqueleto aprovado como template fixo para o post-mortem final.

---

### Experimento 03 — Post-mortem com esqueleto fornecido pelo usuário

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Segunda modalidade do SoT: o usuário fornece a estrutura e o modelo preenche o conteúdo. Demonstra que SoT funciona nos dois sentidos — modelo gera o esqueleto (Exp. 02) ou usuário fornece o esqueleto (Exp. 03).

#### Prompt usado

```
Com base na análise de incidente gerada acima, crie um documento formal de post-mortem seguindo este esqueleto:

1. Título e Metadados (data, severidade, responsáveis)
2. Resumo Executivo (máximo 3 parágrafos)
3. Timeline Detalhada
4. Análise de Causa Raiz (5 Whys)
5. Impacto ao Negócio
6. Ações Imediatas Tomadas
7. Ações Preventivas
8. Lições Aprendidas

Expanda cada seção sem implementar ainda.
Depois, expanda os tópicos e sub tópicos gerando o documento para compartilhar com a equipe de engenharia.
```

#### Resposta da IA

Post-mortem formal completo seguindo o esqueleto fornecido, com 5 Whys derivados da análise do Experimento 02 e lições aprendidas acionáveis.

#### Output completo da IA

**1. Título e Metadados:**
- Título: Incidente P1 — Indisponibilidade total por esgotamento de conexões no db-primary
- Data: 2024-03-15 | Início: 02:14 | Fim: 02:31 | Duração: 17 minutos
- Severidade: P1 | Responsável técnico: SRE on-call | Facilitador do postmortem: Tech Lead

**2. Resumo Executivo:**
Na madrugada de 2024-03-15 às 02:14, ambos os app-servers perderam conectividade com o banco de dados após esgotar o limite de conexões do db-primary (500/500). O load balancer passou a retornar 503 para todos os clientes até 02:31, totalizando 17 minutos de indisponibilidade.

A causa raiz foi o esgotamento de conexões disponíveis no PostgreSQL, possivelmente originado por connection leak ou spike não esperado de carga. A aplicação tinha mecanismo de retry (3 tentativas), mas não tinha circuit breaker para proteger o banco em cenário de saturação.

A resolução foi o reinício do connection pool no db-primary, que liberou conexões imediatamente (45/500). Isso indica que as conexões estavam "presas" e não sendo liberadas corretamente.

**3. Timeline Detalhada:**
*(timestamps precisos extraídos dos logs — idêntica ao Experimento 02)*

**4. Análise de Causa Raiz (5 Whys):**
1. Por que o serviço ficou indisponível? → Load balancer retornando 503
2. Por que o load balancer retornou 503? → App-servers com estado "degraded"
3. Por que os app-servers ficaram degradados? → Timeouts de conexão com db-primary após 3 tentativas
4. Por que os timeouts ocorreram? → db-primary atingiu max_connections (500/500)
5. Por que o limite foi atingido? → Conexões não estavam sendo liberadas corretamente (connection leak ou ausência de pool sizing adequado)

**5. Impacto ao Negócio:**
17 minutos de indisponibilidade no período de menor tráfego (02h). Dependendo do SLA contratado (ex: 99,9% = max 43 min/mês), este incidente pode ter consumido ~40% do budget mensal de downtime.

**6. Ações Imediatas Tomadas:**
- Reinício do connection pool no db-primary (02:31:22)
- Verificação de saúde dos app-servers e load balancer
- Abertura de P1 via PagerDuty (02:15:10)

**7. Ações Preventivas:**
- Implementar PgBouncer para connection pooling externo
- Configurar alerta de connections > 80% do limite
- Implementar circuit breaker na aplicação (Resilience4j ou similar)
- Revisar configuração de `max_connections` no PostgreSQL
- Adicionar métricas de conexões ao dashboard de monitoramento

**8. Lições Aprendidas:**
- Retry sem circuit breaker amplifica o problema: os 3 app-servers continuaram tentando conectar ao banco saturado
- Monitoramento de conexões ativas no banco é tão crítico quanto monitoramento de CPU/memória
- Incidentes de madrugada com tráfego baixo podem mascarar problemas de connection management que explodem em horário de pico

#### O que funcionou

- 5 Whys derivados corretamente dos dados dos logs — causa raiz chegou ao nível de "connection leak" sem estar explícita nos logs
- Lição sobre "retry sem circuit breaker amplifica o problema" emergiu do raciocínio, não estava no prompt
- Documento pronto para compartilhar com a equipe sem edição adicional

#### O que não funcionou

- Seção de impacto ao negócio sem dados reais de SLA — estimativa genérica
- "Responsável técnico" e "Facilitador" ficaram com placeholders — em uso real precisariam de nomes reais

#### Ajustes que eu fiz

- Em uso real: fornecer no prompt "SLA contratado: 99,95%" e "responsável on-call: [nome]" para preencher os campos de metadados automaticamente.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Segunda modalidade do SoT (esqueleto fornecido pelo usuário) funcionou perfeitamente. O modelo preencheu o template formal sem desviar da estrutura. A lição sobre circuit breaker foi emergente e valiosa. Documento pode ser enviado diretamente para a equipe.

#### O que eu faria diferente

Combinar Exp. 02 e 03 em um pipeline: Exp. 02 gera o esqueleto automaticamente a partir dos logs → usuário valida/edita → Exp. 03 preenche o conteúdo final. Isso torna o SoT um processo de dois passos interativo em vez de sequencial fixo.
