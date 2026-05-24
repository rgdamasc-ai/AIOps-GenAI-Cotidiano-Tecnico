# Self-Consistency Prompting

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

## Caderno de Experimentos

---

### Experimento 01 — Diagnóstico do carro (3 mecânicos independentes)

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de Self-Consistency em diagnóstico cotidiano: 3 mecânicos independentes analisam o mesmo problema (barulho metálico ao frear) para identificar convergência (confiança alta) e divergência (investigar mais).

#### Prompt usado

```
Meu carro está fazendo um barulho metálico ao frear, principalmente em baixa velocidade. O carro tem 4 anos e 60.000 km rodados. Nunca troquei as pastilhas.

Gere 3 diagnósticos independentes, como se fossem 3 mecânicos diferentes analisando o problema separadamente. Cada mecânico deve raciocinar passo a passo e chegar ao seu próprio diagnóstico principal, sem considerar a opinião dos outros.

Depois, compare os 3 diagnósticos e indique: onde convergem (confiança alta) e onde divergem (precisa investigar mais).
```

#### Resposta da IA

3 diagnósticos independentes com raciocínio distinto; convergência clara em pastilhas desgastadas como causa principal; divergência no diagnóstico secundário (disco vs caliper).

#### Output completo da IA

**Mecânico 1:**
Raciocínio: 4 anos + 60.000 km + nunca trocou pastilhas → pastilhas no limite de desgaste. Barulho metálico em baixa velocidade ao frear é o indicador clássico. Pastilha raspando no disco.
Diagnóstico: **Pastilhas desgastadas** (confiança alta). Trocar imediatamente.

**Mecânico 2:**
Raciocínio: 60.000 km sem manutenção de freios é incomum. Barulho metálico pode ser pastilha atingindo o indicador sonoro de desgaste, mas também pode ser disco com variação de espessura (DTV) causando vibração. Verificar pedal pulsante.
Diagnóstico: **Pastilhas desgastadas + possível disco com DTV** (confiança média). Inspecionar os dois.

**Mecânico 3:**
Raciocínio: Barulho metálico em baixa velocidade especificamente sugere pastilha no metal, mas pode ser caliper preso liberando lentamente. Caliper preso também causa desgaste desigual das pastilhas.
Diagnóstico: **Pastilhas desgastadas** (confiança alta) + **caliper travado como causa secundária** (verificar).

**Comparação:**
- Convergência (confiança alta): todos os 3 apontam pastilhas desgastadas como diagnóstico principal
- Divergência (investigar mais): Mecânico 2 suspeita de disco com DTV; Mecânico 3 suspeita de caliper preso
- Ação: trocar pastilhas com certeza; inspecionar disco e caliper na mesma revisão

#### O que funcionou

- Convergência em pastilhas deu confiança alta sem ambiguidade — ação clara
- Divergência em disco/caliper identificou pontos de investigação adicional que um mecânico isolado poderia ignorar
- Raciocínio passo a passo de cada mecânico tornou o diagnóstico auditável

#### O que não funcionou

- Em uma conversa única, o modelo pode "lembrar" do raciocínio anterior e enviesar os diagnósticos — o prompt alerta sobre isso mas não elimina completamente
- Sem dados adicionais (pedal pulsante, desgaste visual), a divergência disco/caliper não pode ser resolvida pelo modelo

#### Ajustes que eu fiz

- Para maximizar a independência real: executar cada mecânico em conversas separadas e comparar manualmente. Isso elimina o viés de contexto da conversa única.

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** Self-Consistency funcionou para o objetivo principal: convergência clara = ação imediata (pastilhas), divergência = investigação adicional (disco/caliper). Desconto 1 ponto pelo risco de viés em conversa única.

#### O que eu faria diferente

Executar cada mecânico em 3 conversas separadas para maximizar independência real dos diagnósticos. A convergência resultante seria mais robusta por não ter sido influenciada pelo contexto compartilhado da mesma sessão.

---

### Experimento 02 — Estratégia de migração com 3 perfis profissionais

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Teste de Self-Consistency técnico: Developer, Sysadmin e Arquiteto Cloud analisam o mesmo cenário de migração de aplicação monolítica para AWS de forma independente. Convergência espontânea entre perspectivas distintas é o sinal mais robusto da técnica.

#### Prompt usado

```
Temos uma aplicação monolítica em Java (Spring Boot), rodando em 2 servidores on-premise, com PostgreSQL.
Equipe de 3 devs, prazo de 4 meses, budget limitado. A aplicação tem 80k usuários/mês e picos sazonais.

Precisamos migrar pra AWS. Gere 3 análises independentes dos seguintes perfis:
- Developer: foco em código, frameworks, refatoração e ciclo de desenvolvimento
- Sysadmin: foco em infraestrutura, operação, monitoramento e estabilidade
- Arquiteto Cloud: foco em serviços gerenciados, escalabilidade e custo a longo prazo

Cada profissional deve raciocinar considerando equipe, prazo, budget e perfil da aplicação.
Não considere as outras analises.

Depois, compare as 3 recomendações e indique: onde convergem e onde divergem.
```

#### Resposta da IA

3 perspectivas independentes com raciocínios distintos; convergência em Lift-and-Shift como estratégia inicial; divergência no prazo de evolução e escolha de banco de dados.

#### Output completo da IA

**Developer:**
Raciocínio: 3 devs + 4 meses + monolítico = sem tempo para refatoração profunda. Lift-and-Shift em EC2 com RDS PostgreSQL preserva o código existente. Após a migração, avaliar modularização gradual. Prioridade: manter o CI/CD funcionando e não quebrar o deploy.
Recomendação: **EC2 + RDS**, migração direta, refatoração pós-estabilização.

**Sysadmin:**
Raciocínio: 2 servidores on-premise → EC2. PostgreSQL → RDS Multi-AZ para HA. Picos sazonais exigem Auto Scaling Group. Budget limitado favorece instâncias reservadas. Monitoramento com CloudWatch + alertas de CPU/memória.
Recomendação: **EC2 com ASG + RDS Multi-AZ**, foco em estabilidade e HA.

**Arquiteto Cloud:**
Raciocínio: Budget limitado e prazo curto descartam microserviços. Lift-and-Shift é o caminho correto para o prazo. Porém, após migração, avaliar ECS para containerização e Aurora PostgreSQL para melhor custo/performance no longo prazo.
Recomendação: **EC2 + RDS** na fase 1, roadmap para ECS + Aurora na fase 2.

**Comparação:**
- Convergência (confiança alta): todos os 3 recomendam Lift-and-Shift com EC2 + RDS como estratégia para o prazo de 4 meses
- Divergência: Sysadmin prioriza RDS Multi-AZ (HA) vs Arquiteto que pensa em Aurora no futuro; Developer não menciona HA
- Ação: Lift-and-Shift com EC2 + RDS Multi-AZ (convergência + HA do Sysadmin). Roadmap ECS + Aurora pós-migração (visão do Arquiteto).

#### O que funcionou

- Convergência espontânea em Lift-and-Shift entre 3 perspectivas técnicas distintas — confiança alta
- Developer identificou risco de CI/CD que outros não mencionaram
- Sysadmin adicionou Multi-AZ que outros não priorizaram
- Arquiteto trouxe roadmap de longo prazo sem comprometer o prazo imediato

#### O que não funcionou

- Developer não considerou HA — perspectiva incompleta para decisão de produção
- Budget não foi quantificado — "budget limitado" é subjetivo e cada perfil interpretou diferente

#### Ajustes que eu fiz

- Em uso real: fornecer o budget exato em dólares/mês para que cada perfil calcule a viabilidade de Multi-AZ vs. instância única, tornando a convergência/divergência mais precisa.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Self-Consistency funcionou perfeitamente: convergência espontânea em Lift-and-Shift entre Developer, Sysadmin e Arquiteto eliminou a dúvida sobre a estratégia. Cada perspectiva adicionou um componente que os outros não tinham, tornando a decisão final mais robusta.

#### O que eu faria diferente

Adicionar um quarto perfil — "Gerente de Projeto" — com foco em riscos de cronograma, dependências e comunicação com stakeholders. Isso completaria a perspectiva de negócio que os três perfis técnicos não cobriram.

---

### Experimento 03 — War room de incidente em produção

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de Self-Consistency mais complexo: simulação de war room com Desenvolvedor Sênior, Sysadmin Sênior e Arquiteto Cloud analisando os mesmos logs de API degradada de forma independente.

#### Prompt usado

```
Incidente em produção — API REST com latência degradada.

[logs com HikariPool exausto, slow queries em SELECT * FROM orders JOIN order_items, CPU do RDS em 94% e deploy realizado 2 horas antes]

Gere 3 diagnósticos independentes:
- Desenvolvedor sênior: foco em código, queries e o deploy recente
- Sysadmin sênior: foco em recursos, capacidade e connection pool
- Arquiteto Cloud: foco em design do sistema e gargalos arquiteturais

Cada profissional raciocina passo a passo sem considerar as outras análises.
Depois, compare e indique convergência e divergência.
```

#### Resposta da IA

3 diagnósticos independentes com hipóteses distintas que convergem na causa raiz; cada profissional identificou um aspecto crítico que os outros não priorizaram.

#### Output completo da IA

**Desenvolvedor Sênior:**
Raciocínio: Deploy 2 horas atrás com nova feature de relatórios. Slow query em `SELECT * FROM orders JOIN order_items` com 4.2s — query sem otimização. A nova feature de relatórios provavelmente introduziu esta query sem índice adequado. CPU do RDS em 94% confirma query intensiva em CPU.
Hipótese: **Query não otimizada na feature de relatórios** → pool esgotado como consequência.

**Sysadmin Sênior:**
Raciocínio: HikariPool exausto + RDS com 195/200 conexões + CPU 94% = tríade clássica de saturação. Thread pool em 48/50 — aplicação chegando no limite. A causa pode ser a query lenta, mas o sintoma imediato é pool esgotado.
Hipótese: **Connection pool subdimensionado para a carga atual** + query lenta como amplificador.

**Arquiteto Cloud:**
Raciocínio: Padrão N+1 ou query sem paginação em feature de relatórios é clássico. RDS db.r5.large com max_connections=200 é adequado, mas 3 containers cada um com pool próprio = até 200 conexões consumidas apenas pela aplicação. Falta de cache para relatórios é o problema arquitetural subjacente.
Hipótese: **Ausência de cache para queries de relatório** + query não otimizada = pressão no RDS.

**Comparação:**
- Convergência (confiança alta): todos os 3 apontam a feature de relatórios do deploy recente como origem do problema
- Convergência secundária: query lenta → pool esgotado → latência degradada é a cadeia causal aceita pelos 3
- Divergência: Dev foca em otimização de query; Sysadmin foca em pool sizing; Arquiteto foca em cache
- Plano de ação integrado: (1) fix imediato: rollback ou desabilitar endpoint de relatórios; (2) paliativo: aumentar pool e RDS; (3) redesign: cache + query otimizada com índice

#### O que funcionou

- 3 perspectivas independentes convergiram na causa raiz (feature de relatórios) sem se influenciar
- Cada perspectiva trouxe uma dimensão de solução diferente (query, pool, cache) que o outro não havia priorizado
- Plano de ação em 3 camadas (fix imediato, paliativo, redesign) emergiu naturalmente da comparação
- Self-Consistency transformou o war room em um processo estruturado em vez de uma reunião caótica

#### O que não funcionou

- Sem dados de baseline (latência antes do deploy), a afirmação "degradação causada pelo deploy" é hipótese, não certeza
- Divergência na solução (query vs pool vs cache) não foi resolvida — o prompt não pediu consenso, apenas comparação

#### Ajustes que eu fiz

- Para uso em war room real: após a comparação, adicionar: "com base na convergência, os 3 especialistas chegam a um plano de ação consensual com responsável e prazo para cada ação". Isso fecha o loop do Self-Consistency.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Self-Consistency no seu uso mais poderoso. A convergência espontânea em "feature de relatórios" entre Dev, Sysadmin e Arquiteto deu confiança alta para direcionar o rollback imediato. O plano de 3 camadas emergiu da comparação — nenhum especialista isolado teria chegado a esse plano completo.

#### O que eu faria diferente

Adicionar uma Fase 2 explícita: após a comparação, solicitar que os 3 especialistas produzam um plano de ação com owner e SLA para cada ação. Isso transforma o Self-Consistency em um pipeline completo de war room: diagnóstico independente → comparação → plano consensual.
