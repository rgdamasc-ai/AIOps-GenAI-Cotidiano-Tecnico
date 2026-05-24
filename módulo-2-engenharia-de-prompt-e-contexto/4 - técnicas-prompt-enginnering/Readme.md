# Técnicas de Prompt Engineering

Coleção de prompts práticos organizados por técnica. Cada arquivo nesta pasta cobre uma técnica com exemplos do dia a dia e cenários técnicos de DevOps/Cloud/SRE.

---

## Índice

| Técnica | Arquivo | O que demonstra |
|---|---|---|
| Zero-Shot | [zero-shot.md](zero-shot.md) | Tarefa sem exemplos — o modelo usa conhecimento do treinamento |
| Few-Shot | [few-shot.md](few-shot.md) | Ensinar padrão por exemplos antes da tarefa real |
| Chain-of-Thought | [chain-of-though.md](chain-of-though.md) | Raciocínio passo a passo estruturado |
| Tree-of-Thought | [tree-of-though.md](tree-of-though.md) | Múltiplas perspectivas avaliando o mesmo problema |
| Skeleton-of-Thought | [skeleton-of-though.md](skeleton-of-though.md) | Estrutura primeiro, conteúdo depois |
| Self-Consistency | [self-consistancy.md](self-consistancy.md) | Convergência entre análises independentes = confiança |
| Step-Back | [step-back.md](step-back.md) | Princípios fundamentais antes do diagnóstico |

---

# zero-shot

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

# few-shot

---

## Prompt 1 — Escrita de artigo seguindo padrão de estilo

Prompt usado no Passo 1 da demo. Demonstra como o Few-Shot ensina o **estilo** de escrita ao modelo a partir de um único exemplo de referência. Sem o exemplo, o resultado sairia em um tom genérico e formal; com o exemplo, o modelo replica frases curtas, tom opinativo e analogias do cotidiano.

```
Quero que você escreva artigos seguindo o padrão de estilo do exemplo abaixo.

## Exemplo de referência:

Título: Por que Kubernetes não é pra todo mundo

Todo mundo fala de Kubernetes como se fosse a solução pra tudo. Spoiler: não é.

Se você tem 3 serviços rodando num VPS de 20 dólares, colocar Kubernetes no meio é como comprar um caminhão pra levar uma mochila. Funciona? Funciona. Faz sentido? Nem de longe.

O problema não é o Kubernetes — é achar que complexidade é sinal de maturidade. Às vezes o docker compose resolve. Às vezes um script bash resolve. E tá tudo bem.

Use Kubernetes quando a escala justificar. Quando o problema for real, não quando o ego pedir.

## Agora escreva um artigo neste mesmo estilo sobre: "Por que monitoramento não é só instalar Grafana"
```

---

## Prompt 2 — Mensagens de commit no padrão da equipe

Prompt usado na demo de commits semânticos. Com três exemplos no formato `tipo(escopo): descrição`, o modelo aprende o padrão completo de commit e replica para qualquer nova mudança. Sem o Few-Shot, cada commit sairia em um formato diferente.

```
Gere mensagens de commit seguindo o padrão da equipe. Siga os exemplos.

Exemplo 1:
Mudança: Adicionou endpoint de health check
Commit: feat(api): add health check endpoint with liveness and readiness probes

Exemplo 2:
Mudança: Corrigiu timeout na conexão com Redis
Commit: fix(cache): increase Redis connection timeout to 5s

Exemplo 3:
Mudança: Atualizou versão do Go de 1.20 para 1.21
Commit: chore(deps): bump Go version from 1.20 to 1.21

Agora gere:
Mudança: Adicionou retry com backoff exponencial nas chamadas ao serviço de pagamento
```

---

# chain-of-though

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

# tree-of-though

---

## Prompt 1 — Escolha entre 3 ofertas de emprego

Prompt da demo não-técnica (Passo 1). Aplica o padrão "3 consultores de carreira" para uma decisão pessoal com múltiplas dimensões: salário, modelo de trabalho, área, risco, networking. Mostra como ToT estrutura o "depende do contexto".

```
Imagine três consultores de carreira experientes avaliando minha situação. Cada um apresenta sua análise considerando as três ofertas, debatem os trade-offs entre si, e chegam a uma recomendação consensual.

Contexto: 28 anos, moro sozinho, prefiro trabalho remoto, quero crescer na área de IA, preciso de estabilidade financeira no curto prazo, 2 anos de experiência em Cloud.

Ofertas:
- Empresa A: startup, R$8k, 100% remoto, área de IA, stock options, risco alto
- Empresa B: grande empresa, R$12k, híbrido 3x semana, área de DevOps, estabilidade alta
- Empresa C: consultoria, R$10k, presencial + viagens, múltiplos projetos, networking intenso
```

---

## Prompt 2 — Estratégia de migração para AWS

Prompt da Demo Técnica 1 (Passo 2). Avalia três estratégias clássicas de migração (Lift-and-Shift, Replatform, Refactor) considerando restrições reais de equipe, prazo e tolerância a downtime. Mostra como o ToT torna **auditável** por que cada alternativa foi descartada.

```
Avalie três estratégias de migração para nosso sistema de e-commerce (3 servidores web, PostgreSQL, processamento síncrono, pico na Black Friday) para AWS:

Estratégia 1: Lift-and-Shift — EC2 + RDS, migração direta sem mudanças de arquitetura
Estratégia 2: Replatform — Containers no ECS + RDS + SQS para processamento assíncrono de pedidos
Estratégia 3: Refactor — Serverless com Lambda + API Gateway + DynamoDB

Para cada estratégia, analise:
1. Como resolve o problema de pico na Black Friday
2. Principais vantagens para este contexto
3. Principais riscos e limitações
4. Complexidade de migração (baixa / média / alta)

Após avaliar as três, recomende a melhor considerando: equipe de 4 engenheiros, prazo de 3 meses, sem downtime tolerável.
```

---

## Prompt 3 — Estratégia de escalabilidade com 3 especialistas

Prompt da Demo Técnica 2 (Passo 3) — o mais robusto da aula. Aplica o padrão "3 experts" completo: Dev, Cloud e SRE × 3 estratégias × 5 critérios. Cada especialista traz um risco que os outros não enxergam — é o ToT simulando uma reunião de arquitetura.

```
Imagine três especialistas seniores avaliando a seguinte decisão de escalabilidade. Cada um apresenta sua análise, debatem os trade-offs entre si, e chegam a uma recomendação consensual.

- Especialista em Desenvolvimento: foco em complexidade de código, refatoração necessária, impacto no time de devs e débito técnico
- Especialista em Cloud: foco em serviços AWS, custos, arquitetura de infra e limites de plataforma
- Especialista SRE: foco em operação, observabilidade, resiliência, SLOs e resposta a incidentes

Contexto: Nossa API REST está em um EC2 m5.xlarge (4 vCPUs, 16GB RAM). Atual: 1.000 req/s, latência p99 de 120ms. Meta: 10.000 req/s com p99 < 200ms. Budget: $3.000/mês.

Estratégias a avaliar:

Estratégia 1: Vertical — upgrade para c5.4xlarge (16 vCPUs, 32GB) ou r5.4xlarge
Estratégia 2: Horizontal — Auto Scaling Group com instâncias menores + ALB + externalização de sessão
Estratégia 3: Serverless — migrar para Lambda + API Gateway com concorrência provisionada

Para cada estratégia, os três especialistas devem analisar:
1. Viabilidade técnica — consegue atingir 10.000 req/s com p99 < 200ms?
2. Custo — cabe no budget de $3.000/mês?
3. Complexidade de implementação — esforço pra migrar do estado atual
4. Riscos operacionais — o que pode dar errado em produção
5. Escalabilidade futura — tem espaço pra crescer além dos 10k req/s?

Cada especialista analisa as três estratégias sob sua perspectiva usando esses critérios, debatem os pontos de conflito, e chegam a uma recomendação final com justificativa.
```

---

# skeleton-of-though

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

# self-consistancy

**Importante**: para resultados mais fiéis à técnica, execute cada análise em **conversas separadas** ou peça explicitamente para o modelo não enviesar as respostas entre si.

---

## Prompt 1 — Diagnóstico do carro (3 mecânicos independentes)

Prompt da demo não-técnica (Passo 1). Aplica a lógica de "buscar segunda opinião" a um diagnóstico cotidiano. Mostra a essência do Self-Consistency: convergência = confiança, divergência = investigação adicional.

```
Meu carro está fazendo um barulho metálico ao frear, principalmente em baixa velocidade. O carro tem 4 anos e 60.000 km rodados. Nunca troquei as pastilhas.

Gere 3 diagnósticos independentes, como se fossem 3 mecânicos diferentes analisando o problema separadamente. Cada mecânico deve raciocinar passo a passo e chegar ao seu próprio diagnóstico principal, sem considerar a opinião dos outros.

Depois, compare os 3 diagnósticos e indique: onde convergem (confiança alta) e onde divergem (precisa investigar mais).
```

---

## Prompt 2 — Estratégia de migração com 3 perfis profissionais

Prompt da Demo Técnica 1 (Passo 2). Três perfis (Developer, Sysadmin, Arquiteto Cloud) analisam o mesmo cenário de migração de forma independente. A convergência espontânea entre perspectivas diferentes é o sinal mais robusto de Self-Consistency.

```
Temos uma aplicação monolítica em Java (Spring Boot), rodando em 2 servidores on-premise, com PostgreSQL.
Equipe de 3 devs, prazo de 4 meses, budget limitado. A aplicação tem 80k usuários/mês e picos sazonais.

Precisamos migrar pra AWS. Gere 3 análises independentes dos seguintes perfis, cada um avaliando o cenário separadamente:

- Developer: foco em código, frameworks, refatoração e ciclo de desenvolvimento
- Sysadmin: foco em infraestrutura, operação, monitoramento e estabilidade
- Arquiteto Cloud: foco em serviços gerenciados, escalabilidade e custo a longo prazo

Cada profissional deve raciocinar passo a passo considerando:
- Equipe
- Prazo
- Budget
- Perfil da aplicação
- Recomendar a melhor estratégia de migração

Não considere as outras analises.

Depois, compare as 3 recomendações e indique: onde convergem (confiança alta) e onde divergem (precisa investigar mais).
```

---

## Prompt 3 — War room de incidente em produção

Prompt da Demo Técnica 2 (Passo 3) — o mais poderoso da aula. Simula um war room real: três profissionais diferentes analisando os mesmos logs sob ângulos distintos. A convergência entre Dev, Sysadmin e Arquiteto não só dá a causa raiz, mas o plano de ação completo (fix imediato + paliativo + redesign).

```
Incidente em produção — API REST com latência degradada.

Cenário:
- API REST em Java (Spring Boot) rodando em 3 containers no ECS
- Banco de dados PostgreSQL no RDS (db.r5.large, max_connections=200)
- Deploy realizado há 2 horas com nova feature de relatórios
- Latência normal: p95 ~200ms. Latência atual: p95 ~5.000ms
- Alerta disparou há 30 minutos

Logs:
[2024-03-15 14:32:10] WARN  app-01: Slow query detected: SELECT * FROM orders JOIN order_items ON... (duration: 4.2s)
[2024-03-15 14:32:15] WARN  app-02: Slow query detected: SELECT * FROM orders JOIN order_items ON... (duration: 3.8s)
[2024-03-15 14:32:18] WARN  app-03: HikariPool - Connection pool exhausted, waiting for available connection
[2024-03-15 14:32:22] ERROR app-01: Request timeout after 5000ms - GET /api/v2/reports/monthly
[2024-03-15 14:32:25] WARN  app-02: HikariPool - Connection pool exhausted, waiting for available connection
[2024-03-15 14:32:30] INFO  rds-monitor: CPU utilization at 94%, active connections: 195/200
[2024-03-15 14:32:35] ERROR app-03: Request timeout after 5000ms - GET /api/v2/reports/monthly
[2024-03-15 14:32:40] WARN  app-01: Thread pool near capacity: 48/50 active threads
[2024-03-15 14:32:45] ERROR alb: 5xx errors spike — 23% of requests returning 503
[2024-03-15 14:33:00] INFO  on-call: PagerDuty alert triggered — P1 incident opened

Gere 3 diagnósticos independentes dos seguintes perfis, cada um analisando os logs separadamente:

- Desenvolvedor sênior: foco em código, queries, lógica de aplicação e o deploy recente
- Sysadmin sênior: foco em recursos, capacidade, connection pool e saúde da infraestrutura
- Arquiteto Cloud: foco em design do sistema, gargalos arquiteturais e padrões de escalabilidade

Cada profissional deve raciocinar passo a passo e chegar à sua hipótese principal para a causa raiz, sem considerar as outras analises.

Depois, compare os 3 diagnósticos e indique: onde convergem (confiança alta) e onde divergem (precisa investigar mais).
```

---

# step-back

**Dica prática**: você pode comparar a qualidade da resposta executando o mesmo cenário com e sem a parte do Step-Back, para sentir a diferença de profundidade na análise.

---

## Prompt 1 — Compra de carro com princípios de TCO

Prompt da demo não-técnica (Passo 1). Antes de recomendar um modelo, o modelo é forçado a mapear os princípios de Total Cost of Ownership (TCO) — depreciação, consumo, manutenção, seguro, revenda. Resultado: recomendação baseada em fundamentos, não em popularidade.

```
Preciso comprar um carro com orçamento de até R$100.000. É pra uso urbano, faço uns 30km por dia no trânsito de São Paulo. Qual carro você recomenda?

Antes de recomendar, dê um passo atrás e responda:
Quais são os princípios fundamentais para avaliar o custo total de propriedade (TCO) de um veículo para uso urbano? Considere depreciação, consumo, manutenção, seguro e revenda.

Depois, use esses princípios para recomendar o melhor carro para o meu cenário. Considere carro a combustão e elétrico
```

---

## Prompt 2 — Diagnóstico de CrashLoopBackOff com princípios de memória em containers

Prompt da Demo Técnica 1 (Passo 2). Pod em CrashLoopBackOff com OOMKilled. Antes de diagnosticar, o modelo abstrai os princípios de gerenciamento de memória em K8s (requests vs limits, OOM Killer, cgroups). Resultado: o modelo questiona se é under-provisioning ou memory leak em vez de simplesmente sugerir aumentar o limit.

```
Um pod payment-service está em CrashLoopBackOff no Kubernetes. Os logs mostram:

Container killed due to OOMKilled (exit code 137)
Last state: terminated with exit code 137
Restart count: 5
Resource limits: memory 256Mi
Current usage before crash: 312Mi

Antes de diagnosticar, dê um passo atrás e responda:
Quais são os princípios fundamentais de gerenciamento de memória em containers Kubernetes? Como requests, limits e OOM Killer interagem?

Depois, use esses princípios para diagnosticar o problema e sugerir soluções.
```

---

## Prompt 3 — Conectividade entre serviços com princípios de rede em VPC

Prompt da Demo Técnica 2 (Passo 3). Dois serviços em subnets diferentes de uma mesma VPC AWS não se conectam, mas o ping funciona. Antes de diagnosticar, o modelo mapeia as camadas de controle de tráfego em VPC. Resultado: diagnóstico sistemático por camada, eliminando hipóteses em vez de tentativa e erro.

```
O serviço order-service (subnet privada 10.0.1.0/24) não consegue se conectar ao serviço inventory-service (subnet privada 10.0.2.0/24) na porta 8080. Ambos estão na mesma VPC. O security group do inventory-service permite inbound na porta 8080 de 10.0.1.0/24. O ping entre as instâncias funciona normalmente.

Antes de diagnosticar, dê um passo atrás e responda:
Quais são as camadas de controle de tráfego em uma VPC AWS? Como Security Groups, NACLs, Route Tables e DNS resolution interagem em comunicação entre subnets?

Depois, use esses princípios para diagnosticar o problema de forma sistemática.
```
