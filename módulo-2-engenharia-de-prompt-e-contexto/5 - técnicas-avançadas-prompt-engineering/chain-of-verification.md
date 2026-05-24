# Chain of Verification

Os prompts abaixo demonstram o ciclo completo de Chain of Verification (CoVe) em duas demos: planejamento de churrasco (exemplo do cotidiano) e validação de configuração Terraform (cenário técnico). Cada ciclo tem 4 passos: gerar resposta inicial → gerar perguntas de verificação → responder em janela isolada → revisar e corrigir.

**Importante:** os Passos 1, 2 e 4 rodam na mesma janela (Chat 1). O Passo 3 roda em uma janela nova (Chat 2), sem acesso à resposta original. Esse isolamento é o que evita viés de confirmação.

---

## Demo 1 — Planejamento de Churrasco (Passo 1: plano inicial)

Resposta inicial (baseline). Executado no Chat 1. O cenário tem restrições explícitas — vegetarianos, crianças, churrasqueira pequena, orçamento — que serão checadas nos passos seguintes.

```
Estou organizando um churrasco para 20 pessoas no próximo sábado.
O grupo inclui 3 vegetarianos e 2 crianças. O churrasco começa
às 12h e o espaço tem apenas uma churrasqueira pequena (4 espetos).
Orçamento máximo: R$800.

Monte um plano completo: lista de compras com quantidades e preços
estimados, o que preparar na véspera e o cronograma do dia.
```

---

## Demo 1 — Planejamento de Churrasco (Passo 2: perguntas de verificação)

Mesmo Chat 1. O modelo agora gera perguntas sobre pontos críticos do plano que acabou de produzir. A regra-chave está em "formule as perguntas como perguntas sobre planejamento de churrasco, não como perguntas sobre o plano" — isso prepara o terreno para o isolamento do Passo 3.

```
Agora analise o plano que você acabou de gerar e crie perguntas
de verificação sobre os pontos críticos dele.

Crie perguntas cobrindo:
- Quantidades de itens suficientes?
- Restrições alimentares (vegetarianos, crianças)?
- Cronograma viável com a limitação da churrasqueira?
- Orçamento respeitado?
- Algum item essencial esquecido?

IMPORTANTE: Formule as perguntas como perguntas sobre
planejamento de churrasco, não como perguntas sobre o plano.
Elas serão respondidas por alguém que NÃO terá acesso ao plano.

Retorne APENAS as perguntas, sem responder nenhuma delas.
Retorne em formato markdown.
```

---

## Demo 1 — Planejamento de Churrasco (Passo 3: responder em contexto isolado)

Executado em **chat novo** (Chat 2), sem o plano original. É o isolamento crítico do CoVe — o modelo responde com base apenas no conhecimento próprio, sem viés do plano gerado.

```
Responda as seguintes perguntas sobre planejamento de churrasco
para 20 pessoas (3 vegetarianos, 2 crianças, churrasqueira
pequena de 4 espetos, orçamento de R$800):

[colar APENAS as perguntas do Chat 1, sem o plano]

Responda cada pergunta de forma direta e objetiva, com base no
seu conhecimento. Explique brevemente o raciocínio.
Retorne em formato markdown.
```

---

## Demo 1 — Planejamento de Churrasco (Passo 4: revisão e correção)

De volta ao Chat 1 (que já tem plano + perguntas no contexto). Cola APENAS as respostas do Chat 2. O modelo agora compara o que ele mesmo disse no plano original com o que o "especialista independente" respondeu — e corrige.

```
Aqui estão as respostas de verificação feitas por um especialista
independente que NÃO viu o plano original:

[colar as respostas do Chat 2]

Compare essas respostas com o plano que você gerou e identifique:
1. O que está errado ou incompleto no plano original
2. Quais correções precisam ser feitas
3. O plano corrigido e completo

Retorne em formato markdown.
```

---

## Demo 2 — Validação de Terraform (Passo 1: gerar configuração)

Chat 1 novo. Pede o Terraform completo com todos os componentes da arquitetura. Note que o prompt NÃO menciona segurança ou boas práticas — isso é proposital, para que o ciclo CoVe revele as falhas depois.

```
Gere uma configuração Terraform para uma aplicação web na AWS com:
- VPC com subnets públicas e privadas
- RDS PostgreSQL
- EKS cluster com 2 node groups
- Application Load Balancer
- Security groups para cada componente

Gere o código completo e pronto para aplicar.
```

---

## Demo 2 — Validação de Terraform (Passo 2: perguntas de verificação técnicas)

Mesmo Chat 1. Foca em 5 perguntas de sim/não sobre boas práticas — formato curto e binário facilita a checagem no passo isolado.

```
Analise a configuração Terraform que você acabou de gerar e
identifique os 5 pontos mais críticos de segurança e boas
práticas que podem estar errados ou faltando.

Para cada ponto, gere UMA pergunta curta de sim/não.
Exemplo: "RDS PostgreSQL deve ter encryption at rest habilitado?"

Regras:
- Máximo 5 perguntas
- Apenas sim/não, sem explicações
- Foque em segurança e resiliência, não em naming ou tags
- Formule como boas práticas, não sobre o código

Retorne APENAS as perguntas.
```

---

## Demo 2 — Validação de Terraform (Passo 3: responder em contexto isolado)

Chat 2 (janela nova, sem o Terraform original). O modelo responde sobre boas práticas AWS — não sobre o código gerado. Esse é o ponto de não-viés.

```
Responda as seguintes perguntas sobre boas práticas AWS para
uma aplicação web com VPC, RDS PostgreSQL, EKS e ALB:

[colar APENAS as perguntas do Chat 1]

Para cada pergunta responda SIM ou NÃO e uma frase curta
justificando. Máximo 2 linhas por resposta.
```

---

## Demo 2 — Validação de Terraform (Passo 4: revisão e correção do código)

De volta ao Chat 1. Cola as respostas do Chat 2. O modelo compara as boas práticas confirmadas contra o que ele gerou, e devolve o Terraform corrigido com comentários explicando as mudanças.

```
Aqui estão as respostas de verificação feitas por um especialista
independente que NÃO viu o código Terraform:

[colar as respostas do Chat 2]

Compare essas respostas de boas práticas com a configuração
Terraform que você gerou e:
1. Identifique quais recursos ou configurações estão faltando
   ou incorretos
2. Corrija o Terraform para estar em conformidade
3. Adicione comentários explicando as mudanças
4. Retorne o Terraform corrigido

Formato: código Terraform com markdown.
```

---

## Caderno de Experimentos

---

### Experimento 05 — Chain of Verification: Planejamento de Churrasco

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Revisão

#### Contexto

Demonstração do ciclo completo CoVe (4 passos) aplicado ao planejamento de churrasco com restrições explícitas: 20 pessoas, 3 vegetarianos, 2 crianças, churrasqueira de 4 espetos e orçamento de R$800. O isolamento entre Chat 1 e Chat 2 é o diferencial que evita viés de confirmação.

#### Prompt usado

**Chat 1 — Passo 1 (plano inicial):**
```
Estou organizando um churrasco para 20 pessoas no próximo sábado.
O grupo inclui 3 vegetarianos e 2 crianças. O churrasco começa
às 12h e o espaço tem apenas uma churrasqueira pequena (4 espetos).
Orçamento máximo: R$800.

Monte um plano completo: lista de compras com quantidades e preços
estimados, o que preparar na véspera e o cronograma do dia.
```

**Chat 1 — Passo 2 (perguntas de verificação):** Análise dos pontos críticos do plano com instrução explícita de formular as perguntas como "perguntas sobre planejamento de churrasco, não sobre o plano".

**Chat 2 — Passo 3 (contexto isolado):** As perguntas do Passo 2 coladas num chat novo, sem o plano original.

**Chat 1 — Passo 4 (revisão e correção):** Respostas do Chat 2 coladas de volta no Chat 1 para comparação e correção.

#### Resposta da IA

**Passo 1:** Plano com lista de compras (R$767,50), preparação na véspera e cronograma do dia. Carne total: 6,5kg. Cerveja: 2 caixas.

**Passo 2:** 7 perguntas sobre planejamento de churrasco cobrindo quantidade de carne, restrições alimentares, viabilidade da churrasqueira e orçamento.

**Passo 3 (Chat 2 isolado):** Respondeu que 400-500g de carne por adulto = 6,8-8,5kg total para 17 adultos. Cerveja mínima: 2-3 caixas. Churrasqueira de 4 espetos para 20 pessoas requer mínimo 4 rodadas (80-120min de grelha contínua) e serviço em turnos.

**Passo 4:** Identificou gap de carne (até 2kg a menos), cerveja insuficiente (1 caixa), itens esquecidos (saco de gelo, papel alumínio, abridor) e concluiu que o orçamento de R$800 é insuficiente para o plano corrigido (~R$1.040).

#### Output completo da IA

**Output do Passo 1 (plano inicial):**

**Lista de compras:**

| Item | Quantidade | Preço unit. | Total |
|---|---|---|---|
| Picanha | 3kg | R$90/kg | R$270,00 |
| Frango | 2kg | R$20/kg | R$40,00 |
| Linguiça | 1,5kg | R$25/kg | R$37,50 |
| Espetinhos de legumes (vegetarianos) | — | — | R$30,00 |
| Queijo coalho | 500g | R$30/kg | R$15,00 |
| Pão de alho | — | — | R$15,00 |
| Arroz | 2kg | R$8/kg | R$16,00 |
| Farofa | 1kg | — | R$12,00 |
| Salada (tomate, alface, cebola) | — | — | R$25,00 |
| Refrigerante | 6L | R$8/2L | R$24,00 |
| Cerveja | 2 caixas | R$70/cx | R$140,00 |
| Suco para crianças | 2L | R$8/L | R$16,00 |
| Água | 5L | R$3/L | R$15,00 |
| Carvão | 5kg | R$5/kg | R$25,00 |
| Descartáveis | — | — | R$30,00 |
| **Total** | | | **R$767,50** |

**Véspera:** marinar frango, cortar legumes para espetinhos, preparar farofa, gelar bebidas.

**Cronograma:**
- 10h: acender carvão
- 10h30: linguiça na grelha
- 11h: frango
- 11h30: espetinhos de legumes + queijo coalho
- 12h: chegada dos convidados, servir petiscos
- 12h30: picanha
- 13h30: servir almoço principal

---

**Output do Passo 2 (perguntas de verificação — Chat 1):**

1. Qual a quantidade de carne recomendada por pessoa em um churrasco?
2. Quais opções vegetarianas são suficientes para substituir a carne num churrasco?
3. Quantas rodadas de carne são necessárias em uma churrasqueira de 4 espetos para servir 20 pessoas?
4. Uma churrasqueira pequena de 4 espetos consegue preparar carne suficiente para 20 adultos em tempo hábil, com início às 12h?
5. Quais bebidas e quantidades são adequadas para 20 pessoas (incluindo crianças) num churrasco de 4-5 horas?
6. Quais itens essenciais são frequentemente esquecidos no planejamento de churrasco?
7. O orçamento de R$800 é suficiente para um churrasco de 20 pessoas com restrições alimentares?

---

**Output do Passo 3 (respostas — Chat 2 isolado):**

1. **Quantidade de carne:** 400-500g por adulto. Para 17 adultos: 6,8 a 8,5kg de carne total (considerando mistura de cortes).

2. **Opções vegetarianas:** Espetinhos de legumes (pimentão, abobrinha, berinjela), queijo coalho grelhado, cogumelos, milho. Para 3 vegetarianos: 500-600g de opções grelhadas por pessoa é adequado.

3. **Rodadas numa churrasqueira de 4 espetos:** 4 espetos × ~500g = 2kg por rodada. Para 17 adultos × 400g = 6,8kg → mínimo 4 rodadas. Cada rodada: 20-30 minutos → mínimo 80-120 minutos de grelha contínua.

4. **Viabilidade da churrasqueira:** Com início às 12h, não é possível servir todos de uma vez. O serviço precisa ser em turnos. Recomenda-se começar a grelhar às 10h para ter carne disponível às 12h-13h. A churrasqueira é o principal gargalo.

5. **Bebidas:** Para adultos, calcular ~400ml/hora × 15 adultos bebedores × 5h ≈ 30 litros de cerveja (mínimo 2-3 caixas de 24 latas). Refrigerante: 1,5L por não-bebedor. Para 2 crianças: 500ml cada de suco/refrigerante.

6. **Itens frequentemente esquecidos:** Saco de gelo, papel alumínio (para manter carne quente), abridor de garrafa, tábua de corte, facas para servir, guardanapos extras, protetor solar para anfitriões.

7. **R$800 para 20 pessoas:** Apertado. O valor mínimo viável é ~R$40 por pessoa. Picanha premium estoura o orçamento. Recomenda-se misturar cortes mais acessíveis (fraldinha, maminha) com picanha para equilibrar.

---

**Output do Passo 4 (plano revisado — Chat 1):**

**Problemas identificados no plano original:**

1. **Carne insuficiente:** Plano original = 6,5kg. Recomendação = 6,8-8,5kg. Gap: até 2kg a menos.
2. **Cerveja calculada a menos:** Plano = 2 caixas (~8,4L). Recomendação = 2-3 caixas. Risco real de faltar.
3. **Churrasqueira — serviço em turnos:** O plano assumia servir todos às 13h30 simultaneamente. Com 4 espetos, o serviço será em rodadas contínuas — comunicar isso aos convidados.
4. **Itens esquecidos:** Faltam saco de gelo, papel alumínio, abridor e tábua de corte.

**Impacto no orçamento:**

| Ajuste | Custo adicional |
|---|---|
| +1kg picanha | +R$90 |
| +500g frango | +R$10 |
| +1 caixa cerveja | +R$70 |
| Saco de gelo + alumínio + abridor | +R$25 |
| **Total ajustado** | **~R$962,50** |

**Conclusão:** O orçamento de R$800 é insuficiente para o plano corrigido.

**Opções dentro do orçamento:**
- **Opção A:** Substituir picanha por fraldinha (R$35/kg) → economia de ~R$165 → total ~R$797
- **Opção B:** Manter picanha e reduzir cerveja para 2 caixas + pedir que convidados tragam bebida
- **Opção C:** Negociar aumento do orçamento para R$950 e manter o plano corrigido completo

#### O que funcionou

- O isolamento do Chat 2 revelou inconsistências reais que o Chat 1 não detectaria sozinho: quantidade de carne insuficiente e cerveja subdimensionada
- A instrução "formule como perguntas sobre churrasco, não sobre o plano" foi crucial — permitiu que o Chat 2 respondesse sem viés do plano original
- O Passo 4 produziu análise de gap clara com impacto de orçamento calculado

#### O que não funcionou

- O modelo no Chat 1 (Passo 1) não perguntou se os vegetarianos comem queijo — assumiu que sim, o que pode ser inválido para veganos
- O plano original não considerou o tempo de espera dos convidados enquanto a carne grelha em rodadas

#### Ajustes que eu fiz

- Adicionei contexto das restrições (20 pessoas, 3 vegetarianos, 2 crianças, 4 espetos, R$800) no Passo 3 do Chat 2, junto com as perguntas — necessário para que o modelo isolado pudesse responder com precisão

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O CoVe funcionou exatamente como proposto. O isolamento revelou dois problemas reais (quantidade de carne e bebida) que o modelo não havia identificado no plano original. O Passo 4 gerou um plano revisado com alternativas práticas dentro do orçamento.

#### O que eu faria diferente

No Passo 1 adicionaria a pergunta explícita "os vegetarianos são veganos ou apenas não comem carne?" para o modelo incluir ou excluir queijo coalho e laticínios corretamente.

---

### Experimento 06 — Chain of Verification: Validação de Terraform

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Revisão

#### Contexto

Ciclo CoVe aplicado a validação de código Terraform para uma arquitetura AWS completa (VPC, RDS, EKS, ALB). O prompt inicial propositalmente não menciona segurança ou boas práticas — o ciclo é o mecanismo que revela as omissões.

#### Prompt usado

**Chat 1 — Passo 1:**
```
Gere uma configuração Terraform para uma aplicação web na AWS com:
- VPC com subnets públicas e privadas
- RDS PostgreSQL
- EKS cluster com 2 node groups
- Application Load Balancer
- Security groups para cada componente

Gere o código completo e pronto para aplicar.
```

**Chat 1 — Passo 2:** Análise de 5 pontos críticos de segurança e boas práticas em formato sim/não.

**Chat 2 — Passo 3:** As 5 perguntas coladas num chat novo, sem o Terraform.

**Chat 1 — Passo 4:** Respostas do Chat 2 comparadas com o código gerado, com correção do Terraform.

#### Resposta da IA

**Passo 1:** Terraform funcional com VPC, RDS, EKS e ALB. Sem encryption at rest no RDS, sem deletion protection, sem HTTPS no ALB e sem logging habilitado — omissões esperadas dado o prompt vago.

**Passo 2:** 5 perguntas sobre encryption, HTTPS, deletion protection, logging e multi-AZ.

**Passo 3 (Chat 2):** Confirmou SIM para todas as 5 boas práticas como obrigatórias ou altamente recomendadas.

**Passo 4:** Terraform corrigido com todas as 5 melhorias implementadas e comentadas.

#### Output completo da IA

**Output do Passo 1 (Terraform inicial — trechos relevantes):**

```hcl
resource "aws_db_instance" "postgres" {
  identifier        = "prod-postgres"
  engine            = "postgres"
  engine_version    = "15.4"
  instance_class    = "db.t3.medium"
  allocated_storage = 20
  username          = "admin"
  password          = var.db_password
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.rds.id]
  skip_final_snapshot    = true
  # Sem: storage_encrypted, deletion_protection, multi_az, backup_retention_period
}

resource "aws_lb" "main" {
  name               = "app-alb"
  internal           = false
  load_balancer_type = "application"
  subnets            = aws_subnet.public[*].id
  security_groups    = [aws_security_group.alb.id]
  # Sem: access_logs, drop_invalid_header_fields
}

resource "aws_lb_listener" "http" {
  load_balancer_arn = aws_lb.main.arn
  port              = 80
  protocol          = "HTTP"
  # Sem: redirect para HTTPS
  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}
```

---

**Output do Passo 2 (perguntas — Chat 1):**

1. RDS PostgreSQL deve ter encryption at rest habilitado por padrão em produção?
2. O Application Load Balancer deve redirecionar HTTP para HTTPS automaticamente?
3. Instâncias RDS em produção devem ter deletion protection habilitada?
4. Logs de acesso do ALB devem ser habilitados em produção?
5. RDS PostgreSQL em produção deve estar configurado em Multi-AZ para alta disponibilidade?

---

**Output do Passo 3 (respostas — Chat 2):**

1. **SIM** — Encryption at rest é obrigatória para conformidade (LGPD, SOC2, PCI-DSS) e é gratuita no RDS. Sem custo adicional, sem desculpa para omitir.

2. **SIM** — HTTPS é mandatório em produção. O ALB deve ter listener na porta 443 com certificado ACM e o listener 80 deve redirecionar com 301 para HTTPS.

3. **SIM** — Deletion protection previne destruição acidental do banco via `terraform destroy` ou erro humano. Deve ser habilitada em qualquer banco de produção.

4. **SIM** — Logs de acesso do ALB são essenciais para debugging, análise de segurança e compliance. O custo de armazenamento no S3 é mínimo comparado ao valor diagnóstico.

5. **SIM** — Multi-AZ é o padrão para RDS em produção. Garante failover automático em caso de falha de AZ. O custo dobra, mas é o requisito mínimo de disponibilidade.

---

**Output do Passo 4 (Terraform corrigido — trechos):**

```hcl
resource "aws_db_instance" "postgres" {
  identifier        = "prod-postgres"
  engine            = "postgres"
  engine_version    = "15.4"
  instance_class    = "db.t3.medium"
  allocated_storage = 20
  username          = "admin"
  password          = var.db_password

  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.rds.id]

  # Correção 1: encryption at rest obrigatória em produção
  storage_encrypted = true

  # Correção 2: deletion protection previne destruição acidental
  deletion_protection = true

  # Correção 3: Multi-AZ para failover automático
  multi_az = true

  # Correção 4: backups habilitados (retenção de 7 dias)
  backup_retention_period = 7
  skip_final_snapshot     = false
  final_snapshot_identifier = "prod-postgres-final"
}

# Correção 5: HTTPS listener com certificado ACM
resource "aws_lb_listener" "https" {
  load_balancer_arn = aws_lb.main.arn
  port              = 443
  protocol          = "HTTPS"
  ssl_policy        = "ELBSecurityPolicy-TLS13-1-2-2021-06"
  certificate_arn   = aws_acm_certificate.main.arn

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.app.arn
  }
}

# Redirect HTTP → HTTPS
resource "aws_lb_listener" "http_redirect" {
  load_balancer_arn = aws_lb.main.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type = "redirect"
    redirect {
      port        = "443"
      protocol    = "HTTPS"
      status_code = "HTTP_301"
    }
  }
}

# Correção 6: logging do ALB habilitado
resource "aws_lb" "main" {
  name               = "app-alb"
  internal           = false
  load_balancer_type = "application"
  subnets            = aws_subnet.public[*].id
  security_groups    = [aws_security_group.alb.id]

  drop_invalid_header_fields = true

  access_logs {
    bucket  = aws_s3_bucket.alb_logs.id
    prefix  = "alb"
    enabled = true
  }
}
```

#### O que funcionou

- O ciclo CoVe revelou 5 omissões reais que estavam no código gerado (encryption, HTTPS, deletion protection, logs, Multi-AZ)
- O Chat 2 respondeu com segurança sobre boas práticas — sem acesso ao código gerado, sem viés de "o que eu mesmo escrevi está certo"
- O Passo 4 corrigiu o Terraform com comentários explicativos em cada mudança

#### O que não funcionou

- O Passo 1 gerou Terraform sem `backend` configurado (S3 + DynamoDB para state remoto) — omissão não coberta pelas 5 perguntas do Passo 2
- O limite de 5 perguntas no Passo 2 forçou priorização — itens como IRSA para EKS e VPC Flow Logs ficaram de fora

#### Ajustes que eu fiz

- Adicionei o contexto da arquitetura no Passo 3 do Chat 2 (VPC, RDS, EKS, ALB) mesmo sem o código — necessário para que as respostas fossem contextualizadas

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O CoVe é a técnica mais indicada para validação de código de infraestrutura. O isolamento do Chat 2 produziu respostas objetivas sobre boas práticas sem o viés de "eu mesmo escrevi esse código, deve estar certo".

#### O que eu faria diferente

Faria dois ciclos CoVe: o primeiro cobrindo segurança (os 5 pontos do experimento) e o segundo cobrindo operação (state remoto, tagging, módulos reutilizáveis, custo). Um único ciclo com 5 perguntas não consegue cobrir todas as dimensões de qualidade de um Terraform de produção.
