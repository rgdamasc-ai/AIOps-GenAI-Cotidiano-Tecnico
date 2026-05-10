# Experimento 01 — Plano de estudos com meta de certificação

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

## Contexto

Geração de um plano de estudos semanal personalizado para a certificação AWS Solutions Architect Associate (SAA-C03), com prazo de 12 semanas, carga horária definida pelo usuário e meta de score mínimo de 750/1000.

## Prompt usado

```markdown
# Task
Crie um plano de estudos semanal para a certificação AWS Solutions Architect Associate.

# Action
Organize por domínios do exame, distribua os tópicos em 12 semanas, inclua recursos gratuitos e pagos, e reserve as 2 últimas semanas para simulados e revisão.

# Goal
Objetivo: aprovação no exame com score mínimo de 750/1000, estudando no máximo 2 horas por dia durante a semana e 4 horas nos fins de semana.
```

## Resposta da IA

A IA retornou um plano completo de 12 semanas dividido por domínios do exame SAA-C03, com distribuição proporcional ao peso de cada domínio, lista de recursos gratuitos e pagos, tabela de palavras-chave do exame e sugestão de stack de estudo por ~USD 55. O output foi gerado em uma única iteração, sem necessidade de reformulação.

## Output completo da IA

# Plano de Estudos — AWS Solutions Architect Associate (SAA-C03)
### Meta: 750/1000 | Duração: 12 semanas | Carga: ~18h/semana

## Visão Geral

| Métrica | Valor |
|---|---|
| Horas/dia (semana) | 2h |
| Horas/dia (fim de semana) | 4h |
| Total semanal | 18h |
| Total do plano | ~216h |
| Semanas de conteúdo | 10 |
| Semanas de simulado/revisão | 2 |

### Distribuição por Domínio

| Domínio | Peso | Semanas |
|---|---|---|
| 1 — Design Secure Architectures | 30% | 1–3 |
| 2 — Design Resilient Architectures | 26% | 4–6 |
| 3 — Design High-Performing Architectures | 24% | 7–8 |
| 4 — Design Cost-Optimized Architectures | 20% | 9–10 |
| Simulados e Revisão Final | — | 11–12 |

---

## Semanas 1–3 | Domínio 1: Secure Architectures (30%)

**Semana 1 — Identidade e Controle de Acesso**
IAM (users/groups/roles/policies), AWS Organizations + SCPs, IAM Identity Center (SSO), Federação SAML 2.0, Cognito, AWS STS/AssumeRole

**Semana 2 — Redes e Perímetro de Segurança**
VPC (subnets, routing, IGW, NAT), Security Groups vs NACLs, VPC Endpoints (Gateway/Interface), VPC Peering, Transit Gateway, AWS VPN, Direct Connect, PrivateLink

**Semana 3 — Criptografia e Serviços de Segurança**
KMS (CMKs, envelope encryption, key policies), CloudHSM, ACM, WAF, Shield (Standard/Advanced), GuardDuty, Macie, Security Hub, CloudTrail, AWS Config

---

## Semanas 4–6 | Domínio 2: Resilient Architectures (26%)

**Semana 4 — Computação e Alta Disponibilidade**
EC2 (tipos, placement groups), Auto Scaling (target tracking/step/scheduled), ALB vs NLB vs CLB, Route 53 (todas as routing policies)

**Semana 5 — Armazenamento e Banco de Dados Resilientes**
S3 (versionamento, CRR/SRR, lifecycle), RDS Multi-AZ vs Read Replicas, Aurora (Global DB, Serverless v2), DynamoDB (GSI/LSI, Streams, TTL)

**Semana 6 — Desacoplamento e Mensageria**
SQS (Standard/FIFO, DLQ, visibility timeout), SNS (fan-out), EventBridge, Step Functions, Amazon MQ

---

## Semanas 7–8 | Domínio 3: High-Performing Architectures (24%)

**Semana 7 — Serverless, Containers e Entrega de Conteúdo**
CloudFront (behaviors, OAC, Lambda@Edge), Global Accelerator vs CloudFront, API Gateway (REST/HTTP), Lambda (cold start, concorrência, SnapStart), ECS Fargate/EC2, EKS

**Semana 8 — Storage de Performance e Analytics**
EBS (gp3/io2/st1/sc1, multi-attach), EFS vs FSx (Windows/Lustre/NetApp), Storage Gateway, DataSync, Kinesis (Streams vs Firehose), Redshift, EMR, Athena, Glue

---

## Semanas 9–10 | Domínio 4: Cost-Optimized Architectures (20%)

**Semana 9 — Pricing e Otimização de Computação**
EC2 On-Demand/Reserved/Spot/Dedicated, Savings Plans (Compute vs EC2), Compute Optimizer, S3 storage classes, data transfer costs

**Semana 10 — Governança de Custos**
Cost Explorer, AWS Budgets, Trusted Advisor, Cost and Usage Report, ElastiCache (Redis vs Memcached), rightsizing, Auto Scaling econômico
+ **Simulado completo #1** no sábado

---

## Semanas 11–12 | Simulados e Revisão Final

**Semana 11 — Intensivo de Simulados**
- Seg–Sex: revisão de whitepapers (Well-Architected, DR on AWS, Security Best Practices) + cheat sheets TutorialsDojo
- Sáb: Simulado completo #2 (65 questões) + análise de erros
- Dom: Simulado completo #3 (65 questões) + análise de erros
- **Meta:** score >= 75%

**Semana 12 — Revisão Final e Prova**
- Seg–Qui: revisão rápida por domínio + Simulado completo #4 (quinta-feira)
- Sex: revisão dos erros do #4
- Sáb: descanso com leitura leve
- Dom: **DIA DA PROVA**

---

## Recursos

### Gratuitos
| Recurso | Tipo |
|---|---|
| AWS Skill Builder — Exam Prep Official | Curso oficial |
| Andrew Brown — FreeCodeCamp YouTube | Vídeo-aula 18h |
| TutorialsDojo Cheat Sheets | Referência rápida |
| AWS Documentation | Docs oficiais |
| AWS Workshops (workshops.aws) | Labs hands-on |
| AWS Free Tier | Ambiente real |
| Flashcards Anki — AWS SAA | Memorização |

### Pagos
| Recurso | Custo estimado |
|---|---|
| Adrian Cantrill — SAA-C03 (cantrill.io) | ~USD 40 |
| Stephane Maarek — SAA-C03 (Udemy) | ~USD 15–30 |
| TutorialsDojo Practice Exams | ~USD 15 |
| AWS Skill Builder Individual | USD 29/mês |

**Stack recomendada:** Adrian Cantrill + TutorialsDojo + AWS Free Tier = ~USD 55 total.

---

## Palavras-chave do Exame

| Palavra-chave | Serviços a pensar |
|---|---|
| "mais seguro" / "least privilege" | IAM, KMS, VPC Endpoints |
| "alta disponibilidade" / "fault tolerant" | Multi-AZ, ASG, Route 53 Failover |
| "menor latência" | CloudFront, Global Accelerator, DAX, ElastiCache |
| "menor custo" | Spot, S3 IA, Lambda, Reserved Instances |
| "desacoplar" / "decouple" | SQS, SNS, EventBridge |

## O que funcionou

- O Goal com score mínimo (750/1000) e carga horária diária fez a IA calibrar o plano de forma realista (~216h total)
- A IA distribuiu os domínios proporcionalmente ao peso do exame (30%/26%/24%/20%) sem precisar ser instruída
- A tabela de palavras-chave do exame surgiu sem solicitação explícita — informação de alto valor para revisão
- Recursos foram separados por gratuitos/pagos com estimativa de custo

## O que não funcionou

- Nenhum problema identificado — o output foi completo em uma única iteração

## Ajustes que eu fiz

- Nenhum ajuste necessário. O framework T-A-G com o Goal especificando score mínimo e tempo disponível foi suficiente para gerar um plano acionável e realista

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Output completo, estruturado, com distribuição proporcional ao peso dos domínios, recursos com estimativa de custo e tabela de palavras-chave — tudo em uma única chamada, sem reformulação.

## O que eu faria diferente

O plano está ótimo como ponto de partida. Em uma próxima iteração, poderia adicionar ao Goal o contexto de experiência prévia do estudante (ex: "já tenho 2 anos de AWS em produção") para a IA ajustar a profundidade por semana e possivelmente reduzir de 12 para 8 semanas nos domínios já conhecidos.
