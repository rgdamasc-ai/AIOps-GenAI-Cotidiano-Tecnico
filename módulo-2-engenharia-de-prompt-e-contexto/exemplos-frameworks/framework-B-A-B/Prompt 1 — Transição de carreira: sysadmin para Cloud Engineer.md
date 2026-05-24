# Experimento 01 — Transição de carreira: sysadmin para Cloud Engineer

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

## Contexto

Plano de transição de carreira para profissional saindo de sysadmin tradicional (servidores físicos, deploys manuais via SSH, Zabbix) rumo a Cloud Engineer em 12 meses, sem experiência prévia com cloud, containers ou IaC e com inglês intermediário.

## Prompt usado

```markdown
# Before
Hoje trabalho como sysadmin em uma empresa de médio porte. Minha rotina é gerenciar servidores físicos, fazer deploys manuais via SSH, monitorar com Zabbix e apagar incêndios. Não tenho experiência com cloud, containers ou IaC. Meu inglês é intermediário.

# After
Quero estar trabalhando como Cloud Engineer em até 12 meses, com pelo menos uma certificação AWS, domínio básico de Terraform e Kubernetes, e capaz de participar de processos seletivos em empresas que usam cloud nativa.

# Bridge
Crie um plano de transição de carreira em fases mensais, considerando que posso dedicar 2 horas por dia durante a semana e 4 horas nos fins de semana. Inclua certificações recomendadas em ordem e projetos práticos para construir portfólio.
```

## Resposta da IA

A IA entregou um plano estruturado em 5 fases mensais (12 meses), respeitando a disponibilidade de ~18h/semana (~72h/mês). O plano incluiu 4 projetos práticos progressivos, certificações ordenadas por complexidade e uma fase dedicada a inglês técnico e busca ativa de emprego.

## Output completo da IA

---

**Plano de Transição Sysadmin → Cloud Engineer (12 meses)**

**Disponibilidade estimada:**
- Dias úteis: 2h/dia × 5 dias = 10h/semana
- Fins de semana: 4h/dia × 2 dias = 8h/semana
- Total: ~18h/semana | ~72h/mês

---

### FASE 1 — Fundamentos Cloud + Linux Avançado (Meses 1-2)

*Objetivo: Dominar os conceitos fundamentais da AWS e fortalecer a base Linux*

**Semana 1-2 — Linux e Redes (Revisão):**
- Revisão de: Systemd, crontab, iptables básico, troubleshooting de rede
- Recursos: "Linux Command Line" (livro gratuito), tldr.sh

**Semana 3-4 — AWS Conceitos Core:**
- IAM (usuários, grupos, políticas, roles)
- VPC (subnets, security groups, NACLs, Internet Gateway)
- EC2 (tipos de instância, AMIs, key pairs, user data)

**Semana 5-6 — AWS Storage + Database:**
- S3 (buckets, políticas, versionamento, lifecycle rules)
- RDS (provisioned vs serverless, backups, Multi-AZ conceito)
- EFS básico

**Semana 7-8 — Preparação para AWS Cloud Practitioner:**
- Practice exams: ExamTopics, Tutorials Dojo (modo estudo)
- Revisão de todos os serviços cobrados

**Certificação: AWS Cloud Practitioner (CLF-C02)**
- Custo: ~$100 USD
- Dificuldade: Baixa
- Por que começar aqui: Valida fundamentos sem pressão técnica, abre portas e aumenta confiança

---

### FASE 2 — AWS Solutions Architect Associate + IaC com Terraform (Meses 3-5)

*Objetivo: Aprofundar arquitetura AWS e introduzir Infrastructure as Code*

**Mês 3 — Arquitetura AWS:**
- Load Balancers (ALB, NLB), Auto Scaling Groups
- Route 53 (DNS, health checks, routing policies)
- CloudFront básico, S3 Static Website
- CloudWatch (metrics, logs, alarms)
- SNS, SQS conceitos

**Projeto Prático 1: "Deploy de Aplicação Web Resiliente"**
- Criar VPC do zero com subnets públicas/privadas
- EC2 + ALB + Auto Scaling Group
- RDS em subnet privada
- Configurar alertas no CloudWatch
- Documentar no GitHub com diagrama de arquitetura

**Mês 4 — Terraform Fundamentos:**
- HCL syntax: providers, resources, variables, outputs
- State management (local e remote com S3 backend)
- Módulos e boas práticas
- Recursos: HashiCorp Learn (grátis), "Terraform Up & Running" (capítulos 1-4)

**Projeto Prático 2: "Infraestrutura do Projeto 1 em Terraform"**
- Refatorar tudo que foi feito manualmente para Terraform
- Criar módulos para VPC, EC2, RDS
- Usar remote state no S3 com DynamoDB para locking
- README detalhado com instruções

**Mês 5 — Revisão SAA-C03:**
- Foco em well-architected framework (5 pilares)
- Simulados: Tutorials Dojo (2 simulados/semana)
- Revisão de serviços menos familiares: Kinesis, ElastiCache, SQS/SNS patterns

**Certificação: AWS Solutions Architect Associate (SAA-C03)**
- Custo: ~$150 USD
- Dificuldade: Intermediária
- Por que: É a certificação mais reconhecida pelo mercado para Cloud Engineers

---

### FASE 3 — Containers + Kubernetes (Meses 6-8)

*Objetivo: Dominar o ecossistema de containers*

**Mês 6 — Docker Fundamentos:**
- Containers vs VMs (sua experiência como sysadmin ajuda aqui)
- Dockerfile, docker-compose
- Registry (ECR na AWS)
- Redes e volumes Docker

**Projeto Prático 3: "Containerizar uma Aplicação Existente"**
- Pegar um projeto open source simples (ex: Flask app)
- Dockerizar com multi-stage build
- Publicar no ECR
- Fazer deploy manual em EC2

**Mês 7 — Kubernetes Fundamentos:**
- Pods, Deployments, Services, ConfigMaps, Secrets
- Namespaces, Resource Limits
- kubectl básico
- Recursos: killer.sh, kodekloud.com (labs práticos)

**Mês 8 — EKS + CI/CD Básico:**
- AWS EKS (managed Kubernetes)
- Helm Charts básicos
- GitHub Actions: pipeline básico (build, test, push to ECR, deploy to EKS)

**Projeto Prático 4: "Pipeline CI/CD Completo" (carro-chefe do portfólio)**
- App containerizada no EKS
- GitHub Actions fazendo build + push no ECR
- Deploy automático via Helm
- Terraform provisionando o cluster EKS

---

### FASE 4 — Observabilidade + Segurança + Polimento do Portfólio (Meses 9-10)

**Mês 9 — Observabilidade:**
- CloudWatch Logs Insights, métricas customizadas
- Conceitos de SLO/SLI/SLA
- Alertas e dashboards profissionais
- Introdução ao Prometheus/Grafana (muito cobrado em vagas)

**Mês 10 — Segurança e Boas Práticas:**
- IAM avançado (policies com conditions, SCPs no Organizations)
- AWS Security Hub, GuardDuty conceito
- Secrets Manager vs Parameter Store
- Well-Architected Review do portfólio

**Certificação Opcional: CKA (Certified Kubernetes Administrator)**
- Custo: ~$395 USD
- Dificuldade: Alta
- Quando: Apenas se quiser foco específico em Kubernetes

---

### FASE 5 — Inglês Técnico + Busca Ativa de Emprego (Meses 11-12)

**Mês 11 — Inglês Técnico:**
- Daily: ler documentação AWS em inglês
- 30min/dia: falar sobre conceitos técnicos em inglês (gravação de vídeos explicativos)
- Assistir: re:Invent talks no YouTube
- Conversa com comunidades: AWS Community Discord, Reddit r/aws

**Mês 12 — Estratégia de Busca:**
- LinkedIn: otimizar perfil com as certificações e projetos
- GitHub: polir os 4 projetos (READMEs profissionais, diagramas, badges)
- Aplicar para vagas: começar 2 meses antes do prazo desejado
- Comunidades: AWS User Groups Brasil, Meetups DevOps

---

**Resumo de Certificações em Ordem:**
1. AWS Cloud Practitioner (Mês 2) — base
2. AWS Solutions Architect Associate (Mês 5) — essencial para o mercado
3. HashiCorp Terraform Associate (Mês 7, opcional) — diferencial
4. CKA (após os 12 meses, se quiser especializar em K8s)

**Portfólio ao Final dos 12 Meses:**
1. VPC + EC2 + ALB + RDS (manual, para aprendizado)
2. Infraestrutura anterior em Terraform (IaC)
3. App containerizada no ECS/ECR
4. Pipeline CI/CD completo: GitHub Actions → ECR → EKS (Terraform)

**Dica de ouro:** Documente tudo publicamente. Escreva posts no LinkedIn sobre o que está aprendendo. Isso diferencia você de centenas de candidatos com o mesmo currículo.

---

## O que funcionou

- O framework B-A-B gerou um plano altamente estruturado e sequencial, sem pulares etapas lógicas
- A IA respeitou fielmente a restrição de disponibilidade (2h/dia + 4h fim de semana) sem superestimar
- A ordem das certificações foi didaticamente correta: do mais simples ao mais complexo
- Os projetos práticos foram progressivos e cumulativos (cada um usa o anterior como base)
- A IA antecipou um ponto crítico sem ser perguntada: a dica sobre documentar publicamente no LinkedIn
- A experiência de sysadmin foi aproveitada como ponte (ex: "Containers vs VMs — sua experiência ajuda aqui")

## O que não funcionou

- Não houve estimativa de custo dos recursos AWS utilizados nos projetos práticos (pode ser uma surpresa financeira)
- A fase de inglês ficou concentrada apenas no final; idealmente deveria ser paralela desde o início
- Faltou mencionar recursos gratuitos específicos (AWS Free Tier limites, GitHub Student Pack, etc.)

## Ajustes que eu fiz

- Nenhuma reformulação foi necessária — o prompt B-A-B com contexto rico entregou um plano completo na primeira tentativa
- Em uso real, adicionaria ao Bridge: "Considere meu orçamento limitado para certificações e inclua recursos gratuitos prioritariamente"

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O framework B-A-B entregou exatamente o esperado: um plano de 12 meses detalhado, faseado, respeitando as restrições reais do Before e atingindo todos os objetivos do After. A qualidade e completude do output na primeira tentativa demonstra o poder do contexto rico que o B-A-B força o usuário a fornecer.

## O que eu faria diferente

Incluiria no Bridge uma restrição financeira explícita e pediria que a IA sugerisse quando iniciar o inglês técnico em paralelo com o estudo técnico, em vez de deixar para o final. O B-A-B funciona melhor quanto mais específico for o Bridge — restrições bem definidas geram planos mais acionáveis.
