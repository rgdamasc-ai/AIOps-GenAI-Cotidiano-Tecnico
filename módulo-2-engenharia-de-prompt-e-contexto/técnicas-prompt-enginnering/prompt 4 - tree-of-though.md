# Tree-of-Thought Prompting

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

## Caderno de Experimentos

---

### Experimento 01 — Escolha entre 3 ofertas de emprego

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Teste de ToT para decisão pessoal com múltiplas dimensões (salário, modelo de trabalho, área, risco, networking) usando o padrão "3 consultores de carreira" debatendo trade-offs.

#### Prompt usado

```
Imagine três consultores de carreira experientes avaliando minha situação. Cada um apresenta sua análise considerando as três ofertas, debatem os trade-offs entre si, e chegam a uma recomendação consensual.

Contexto: 28 anos, moro sozinho, prefiro trabalho remoto, quero crescer na área de IA, preciso de estabilidade financeira no curto prazo, 2 anos de experiência em Cloud.

Ofertas:
- Empresa A: startup, R$8k, 100% remoto, área de IA, stock options, risco alto
- Empresa B: grande empresa, R$12k, híbrido 3x semana, área de DevOps, estabilidade alta
- Empresa C: consultoria, R$10k, presencial + viagens, múltiplos projetos, networking intenso
```

#### Resposta da IA

Três consultores com perspectivas distintas (financeiro, carreira técnica, lifestyle) avaliaram as ofertas de forma independente e chegaram a recomendação consensual com justificativa dos trade-offs descartados.

#### Output completo da IA

**Consultor 1 — Foco em estabilidade financeira:**
Empresa B é a escolha racional no curto prazo. R$12k cobre reserva de emergência, custo de vida sozinho e ainda permite investimento. Startup com R$8k é risco real — stock options têm valor incerto. Empresa C tem modelo presencial que contradiz a preferência declarada.

**Consultor 2 — Foco em trajetória técnica:**
Empresa A é a única alinhada ao objetivo de longo prazo (IA). Dois anos de experiência em Cloud é base sólida para transição. O diferencial de R$4k entre A e B não justifica perder 2-3 anos em DevOps quando a meta é IA. Stock options em startup de IA com tração podem mudar o cenário financeiro.

**Consultor 3 — Foco em qualidade de vida e networking:**
Empresa C expõe a múltiplos clientes e acelera aprendizado. Mas presencial + viagens é incompatível com a preferência por remoto. Empresa A tem o modelo ideal (100% remoto + área alinhada), mas o risco financeiro é real para quem mora sozinho.

**Debate e consenso:**
Os consultores convergem que Empresa B é a escolha mais prudente no curto prazo por 12-18 meses para consolidar reserva financeira, enquanto a pessoa desenvolve projetos de IA lateralmente (open source, cursos, side projects). Após esse período, reavaliar Empresa A com base financeira mais sólida. Empresa C é descartada pelo modelo de trabalho incompatível.

**Recomendação final:** Empresa B, com plano explícito de transição para IA em 12-18 meses.

#### O que funcionou

- Cada consultor trouxe uma dimensão diferente que os outros não priorizaram
- O debate explicitou por que Empresa C foi descartada (modelo de trabalho)
- Recomendação com horizonte temporal definido (12-18 meses) em vez de resposta binária
- ToT revelou que a decisão não é "A ou B" mas "B agora, A depois"

#### O que não funcionou

- Os consultores não quantificaram o impacto financeiro das stock options (risco/retorno esperado)
- Sem análise de custo de vida sozinho em São Paulo vs salário de R$8k para avaliar viabilidade real

#### Ajustes que eu fiz

- Em uso real: adicionar "Consultor 4 — financeiro: calcule o breakeven das stock options da Empresa A considerando vesting de 4 anos" para completar a análise.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** ToT tornou auditável por que Empresa C foi descartada e por que a decisão não é simples. A recomendação "B agora, A depois" só emergiu do debate — nenhum consultor isolado chegaria a essa conclusão.

#### O que eu faria diferente

Pedir ao final: "cada consultor apresente o maior risco da recomendação consensual" para garantir que a recomendação final também seja auditada pelos próprios consultores que a propuseram.

---

### Experimento 02 — Estratégia de migração para AWS

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Teste de ToT técnico avaliando 3 estratégias de migração (Lift-and-Shift, Replatform, Refactor) para sistema de e-commerce com restrições reais de equipe, prazo e downtime.

#### Prompt usado

```
Avalie três estratégias de migração para nosso sistema de e-commerce (3 servidores web, PostgreSQL, processamento síncrono, pico na Black Friday) para AWS:

Estratégia 1: Lift-and-Shift — EC2 + RDS
Estratégia 2: Replatform — ECS + RDS + SQS
Estratégia 3: Refactor — Serverless Lambda + API Gateway + DynamoDB

Para cada estratégia, analise: resolução do pico Black Friday, vantagens, riscos, complexidade.
Recomende considerando: 4 engenheiros, prazo de 3 meses, sem downtime tolerável.
```

#### Resposta da IA

Análise estruturada das 3 estratégias com matriz de 4 critérios e recomendação justificada, com explicitação de por que cada alternativa foi descartada.

#### Output completo da IA

**Estratégia 1 — Lift-and-Shift (EC2 + RDS):**
- Black Friday: resolve com Auto Scaling no EC2, mas processamento síncrono mantém o gargalo
- Vantagens: menor risco de migração, prazo viável, sem refatoração de código
- Riscos: não resolve o problema fundamental do processamento síncrono; débito técnico mantido
- Complexidade: Baixa

**Estratégia 2 — Replatform (ECS + RDS + SQS):**
- Black Friday: SQS desacopla processamento de pedidos, eliminando o gargalo síncrono
- Vantagens: resolve o problema principal sem refatoração completa; RDS preserva o schema PostgreSQL
- Riscos: adaptação da aplicação para containers e filas requer esforço moderado
- Complexidade: Média

**Estratégia 3 — Refactor (Lambda + DynamoDB):**
- Black Friday: escalabilidade ideal para picos, sem provisionamento
- Vantagens: arquitetura event-driven elimina todos os gargalos
- Riscos: DynamoDB requer migração de schema PostgreSQL; Lambda tem cold start; 3 meses é inviável para equipe de 4
- Complexidade: Alta — inviável no prazo

**Recomendação — Estratégia 2 (Replatform):**
Estratégia 1 não resolve o problema de Black Friday (processamento síncrono permanece). Estratégia 3 é inviável em 3 meses com equipe de 4. Estratégia 2 resolve o gargalo central com complexidade moderada e prazo factível. SQS desacopla pedidos, ECS é mais próximo dos servidores atuais do que serverless.

#### O que funcionou

- Eliminação explícita de Estratégia 3 pelo critério de prazo/equipe — auditável
- Identificação de que Estratégia 1 não resolve o problema central (processamento síncrono)
- Recomendação com justificativa por exclusão, não apenas por preferência
- Critério de "sem downtime tolerável" influenciou o descarte de Estratégia 3

#### O que não funcionou

- Sem estimativa de custo comparativo entre as estratégias
- Sem plano de rollback explícito para cada estratégia
- Riscos do Replatform (containers, filas) listados mas não aprofundados

#### Ajustes que eu fiz

- Em uso real: adicionar "estime o custo mensal de cada estratégia em AWS" e "descreva o plano de rollback se a migração falhar" para completar a análise de decisão arquitetural.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** ToT tornou o processo de decisão arquitetural completamente auditável. Qualquer stakeholder pode entender por que as alternativas foram descartadas sem depender da expertise técnica do time.

#### O que eu faria diferente

Adicionar um critério explícito de "viabilidade com zero downtime" como dimensão separada na análise de cada estratégia, já que esse era o constraint mais crítico do cenário.

---

### Experimento 03 — Estratégia de escalabilidade com 3 especialistas

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Teste de ToT mais complexo: 3 especialistas sênior (Dev, Cloud, SRE) × 3 estratégias × 5 critérios. Simula uma reunião de arquitetura com perspectivas técnicas distintas para meta de 10× de throughput.

#### Prompt usado

```
Imagine três especialistas seniores (Dev, Cloud, SRE) avaliando 3 estratégias de escalabilidade para uma API REST em EC2 m5.xlarge.

Meta: de 1.000 para 10.000 req/s, p99 < 200ms, budget $3.000/mês.

Estratégias: Vertical (c5.4xlarge), Horizontal (ASG + ALB), Serverless (Lambda + API Gateway).

Para cada estratégia, os especialistas analisam: viabilidade técnica, custo, complexidade, riscos operacionais, escalabilidade futura.
```

#### Resposta da IA

Cada especialista trouxe riscos que os outros não identificaram; a recomendação consensual emergiu do debate, não de um especialista isolado.

#### Output completo da IA

**Especialista Dev:**
- Vertical: sem mudança de código, mas teto claro de escala
- Horizontal: exige externalizar sessão (Redis/DynamoDB) — refatoração necessária
- Serverless: reescrever para stateless e cold start impacta p99 — inviável sem refatoração profunda

**Especialista Cloud:**
- Vertical: c5.4xlarge ~$0,68/h = ~$490/mês. Viável no budget, mas sem headroom para picos
- Horizontal: 4× m5.large + ALB ~$600/mês. Dentro do budget com auto-scaling real
- Serverless: Lambda 10k req/s com concorrência provisionada ~$1.200/mês. Cabe, mas sem margem

**Especialista SRE:**
- Vertical: Single point of failure sem mudança. Falha da instância = downtime completo
- Horizontal: Complexidade operacional de health checks, session stickiness e draining
- Serverless: Observabilidade fragmentada (X-Ray vs APM tradicional). Resposta a incidente mais lenta

**Debate:**
- Dev e SRE convergem: Serverless requer refatoração + mudança de modelo operacional — alto risco para meta de curto prazo
- Cloud aponta que Vertical cabe no budget mas SRE veta pelo SPOF
- Consenso em Horizontal: exige refatoração de sessão (Dev aceita), tem custo ok (Cloud confirma), e SRE pode operar com playbooks existentes para ASG

**Recomendação:** Horizontal com ASG + ALB, externalizando sessão para ElastiCache Redis. Viabilidade técnica, custo dentro do budget e operação conhecida pelo SRE.

#### O que funcionou

- Dev identificou a necessidade de refatoração de sessão — não estava no prompt
- SRE apontou o SPOF do Vertical que o Cloud não havia considerado
- Cloud calculou custos que invalidaram a percepção de "Serverless é mais barato"
- Consenso emergiu do debate, não de um especialista dominando

#### O que não funcionou

- Sem análise de tempo de implementação de cada estratégia
- "Reescrever para stateless" do Serverless não foi estimado em sprints/semanas

#### Ajustes que eu fiz

- Em uso real: adicionar "cada especialista estime o tempo de implementação e o maior risco operacional pós-go-live" para tornar a análise diretamente acionável como input de planejamento.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Este é o ToT no seu melhor uso: 3 especialistas identificaram riscos que nenhum deles teria percebido sozinho. O SPOF do Vertical (SRE), a necessidade de refatoração de sessão (Dev) e o cálculo real de custo do Serverless (Cloud) só apareceram porque a perspectiva era múltipla e independente.

#### O que eu faria diferente

Separar em duas conversas: primeira com cada especialista analisando as estratégias de forma realmente independente, depois uma segunda rodada de debate. Isso maximiza a divergência inicial e torna o consenso mais robusto.
