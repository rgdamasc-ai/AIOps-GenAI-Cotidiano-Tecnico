# Experimento 02 — Migração on-premises para AWS

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

## Contexto

Plano de migração faseada de aplicação Python/Django on-premises (NGINX + Gunicorn em 2 VMs, PostgreSQL, NFS, Nagios, deploy manual via SSH, sem CI/CD) para AWS com ECS Fargate, RDS PostgreSQL Multi-AZ, S3, GitHub Actions, CloudWatch e Terraform, sem derrubar a produção durante a migração.

## Prompt usado

```markdown
# Before
Temos uma aplicação web em Python/Django rodando em 2 VMs on-premises (NGINX + Gunicorn), banco PostgreSQL em uma VM dedicada, storage em NFS, e backups manuais semanais em fita. O monitoramento é básico (Nagios verificando uptime). Deploy é manual: o dev faz SSH, puxa o código do Git e reinicia o serviço. Não temos CI/CD. A aplicação atende ~500 usuários internos.

# After
Queremos a aplicação rodando na AWS com: containers em ECS Fargate, banco em RDS PostgreSQL Multi-AZ, storage em S3, pipeline CI/CD com GitHub Actions, monitoramento com CloudWatch e alertas no Slack, backups automáticos diários com retenção de 30 dias, e infraestrutura como código com Terraform.

# Bridge
Crie um plano de migração em fases, onde cada fase possa ser executada sem derrubar a aplicação em produção. Para cada fase inclua: o que será migrado, pré-requisitos, estimativa de esforço, riscos principais e plano de rollback.
```

## Resposta da IA

A IA entregou um plano de migração em 5 fases (mais uma fase 0 de preparação), totalizando 16-21 semanas e ~540h de esforço. Cada fase incluiu o que seria migrado, pré-requisitos, estimativa de esforço, riscos principais e plano de rollback detalhado. A IA manteve o princípio de zero-downtime em todas as fases com estratégias específicas por componente.

## Output completo da IA

---

**Plano de Migração Zero-Downtime: On-Premises Django → AWS**

---

### FASE 0 — Preparação e Fundamentos (2-3 semanas)

**O que será feito:**
- Inventário completo da aplicação (dependências, variáveis de ambiente, secrets)
- Containerização da aplicação Django (Dockerfile + docker-compose local)
- Configuração da conta AWS (Organizations, IAM, budgets de custo)
- Estrutura Terraform: providers, remote state (S3 + DynamoDB), módulos básicos

**Pré-requisitos:**
- Acesso à conta AWS com permissões de administrador
- Repositório Git com o código da aplicação
- Docker instalado nos ambientes de desenvolvimento

**Esforço estimado:** 80h (2 DevOps engineers × 2 semanas)

**Riscos principais:**
- Dependências ocultas não documentadas (arquivos de configuração hardcoded)
- Secrets espalhados em arquivos locais

**Plano de rollback:** Nenhum impacto em produção nesta fase

---

### FASE 1 — VPC + RDS PostgreSQL Multi-AZ (3-4 semanas)

**O que será migrado:**
- VPC com subnets públicas/privadas em 2 AZs
- RDS PostgreSQL Multi-AZ (snapshot inicial + replicação contínua com DMS)
- Security Groups para todo o ambiente

**Pré-requisitos:**
- Fase 0 concluída
- Terraform com módulo de VPC aprovado
- Janela de manutenção acordada para migração inicial do banco

**Esforço estimado:** 120h

**Processo de migração do banco sem downtime:**
1. Criar RDS na AWS (Multi-AZ, mesma versão do PostgreSQL)
2. Usar AWS DMS (Database Migration Service) para replicação contínua
3. DMS sincroniza continuamente: on-premises → RDS
4. Validar integridade dos dados com checksums
5. No dia do cutover: parar aplicação on-prem por ~5 minutos, aguardar DMS sincronizar, apontar connection string para RDS

**Riscos principais:**
- Latência de replicação DMS em tabelas muito grandes (usar parallel load)
- Versão do PostgreSQL incompatível com RDS (validar na Fase 0)

**Plano de rollback:**
- DMS pode ser revertido: manter on-premises como primary até validação completa
- RDS pode ser destruído via Terraform se algo der errado

---

### FASE 2 — Storage NFS → S3 (2-3 semanas)

**O que será migrado:**
- Arquivos do NFS para S3 (media files, uploads)

**Pré-requisitos:**
- Fase 1 concluída
- Bucket S3 criado com versionamento, lifecycle rules e criptografia (SSE-S3)
- django-storages configurado e testado em ambiente de desenvolvimento

**Esforço estimado:** 60h

**Processo:**
1. Instalar django-storages + boto3 na aplicação
2. Configurar settings para usar S3 como backend de media files via variável de ambiente (sem mudar código de produção)
3. Sincronizar NFS → S3 com AWS DataSync (preserva timestamps e permissões)
4. Testar em ambiente staging apontado para S3
5. Ativar S3 em produção via feature flag (variável de ambiente)
6. Manter NFS por 30 dias como backup, depois descomissionar

**Riscos principais:**
- Caminhos hardcoded para NFS no código da aplicação
- Permissões de arquivo que não mapeiam para S3 (S3 não tem sistema de permissões POSIX)

**Plano de rollback:**
- Reverter variável de ambiente para usar NFS (sem redeploy necessário, < 1 minuto)

---

### FASE 3 — Containerização + ECS Fargate (4-5 semanas)

**O que será migrado:**
- Aplicação Django dos VMs para ECS Fargate
- NGINX substituído por Application Load Balancer

**Pré-requisitos:**
- Fases 1 e 2 concluídas
- Dockerfile testado e funcionando localmente
- ECR (Elastic Container Registry) configurado
- Task Definition e ECS Cluster criados via Terraform

**Esforço estimado:** 160h

**Processo:**
1. Criar ECR e fazer push da primeira imagem Docker
2. Criar ECS Cluster (Fargate) via Terraform
3. Task Definition com:
   - Container da aplicação Django + Gunicorn
   - Variáveis de ambiente via Secrets Manager (DATABASE_URL, SECRET_KEY, etc.)
   - Health checks configurados no ALB
4. ALB na frente do ECS (substituindo NGINX on-prem)
5. Configurar Auto Scaling baseado em CPU/Memory
6. DNS: adicionar registro no Route 53 apontando para ALB em paralelo com on-prem (TTL = 60s antecipadamente)
7. Testar exaustivamente em staging
8. Cutover: atualizar DNS principal para ALB
9. Monitorar por 48h antes de descomissionar VMs

**Riscos principais:**
- Sessões de usuário perdidas no redeploy (verificar session backend — migrar para ElastiCache Redis se necessário)
- Jobs Django (Celery, management commands) precisam de ECS Task separada
- Variáveis de ambiente não mapeadas corretamente no Secrets Manager

**Plano de rollback:**
- DNS pode ser revertido em < 2 minutos (TTL baixo previamente configurado)
- VMs on-premises mantidas ativas por 30 dias após o cutover

---

### FASE 4 — CI/CD com GitHub Actions + Observabilidade com CloudWatch (3-4 semanas)

**O que será implementado:**
- Pipeline CI/CD com GitHub Actions
- Monitoramento com CloudWatch + alertas no Slack
- Substituição do Nagios

**Pré-requisitos:**
- Fase 3 concluída
- Repositório no GitHub com branch strategy definida (trunk-based recomendado)
- Slack workspace e webhook configurados

**Esforço estimado:** 80h

**Pipeline GitHub Actions:**
```
Push to main:
  1. Run tests (pytest)
  2. Build Docker image
  3. Push to ECR (tag: git SHA + latest)
  4. Update ECS Task Definition (nova imagem)
  5. Deploy via ECS Rolling Update (sem downtime)
  6. Health check do ALB valida o deploy automaticamente
  7. Notificação no Slack (sucesso/falha com link para o deploy)
```

**Observabilidade:**
- CloudWatch Logs: logs da aplicação Django centralizados via log group
- CloudWatch Metrics: CPU, Memory, Request Count, 5xx errors por Target Group
- Dashboard CloudWatch: visão geral da aplicação em tempo real
- Alertas configurados: 5xx > 1%, CPU > 80%, Memory > 80% → notificação Slack
- Substituição do Nagios: CloudWatch Health Checks + Route 53 Health Checks

**Riscos principais:**
- IAM Role do GitHub Actions com permissões excessivas (usar OIDC, não access key estático)
- Logs sensíveis (PII) enviados ao CloudWatch sem mascaramento

**Plano de rollback:**
- ECS suporta rollback automático se health check falhar durante Rolling Update

---

### FASE 5 — Backups Automáticos + Hardening de Segurança (2 semanas)

**O que será implementado:**
- Backup automático do RDS (daily, retenção 30 dias)
- Lifecycle policy do S3 para cold storage
- AWS Backup para política centralizada
- Revisão de segurança final

**Pré-requisitos:**
- Todas as fases anteriores concluídas
- Runbook de restauração rascunhado

**Esforço estimado:** 40h

**Backups:**
- RDS: automated backups habilitado (daily, janela 02:00-04:00, 30 dias de retenção)
- RDS: manual snapshot obrigatório antes de cada deploy major
- S3: versioning habilitado + lifecycle rule (Standard → IA após 30 dias → Glacier após 90 dias)
- AWS Backup: policy centralizada com tags de backup para todos os recursos
- Testar restauração mensalmente (runbook documentado e executado em conta de staging)

**Hardening:**
- Security Groups: princípio do menor privilégio revisado
- IAM: remover permissões de AdministratorAccess usadas durante a migração
- Secrets Manager: rotação automática de credenciais habilitada
- GuardDuty: habilitado para detecção de ameaças

---

**Cronograma Resumido:**

| Fase | Descrição | Duração | Esforço |
|------|-----------|---------|---------|
| 0 | Preparação e Fundamentos | 2-3 semanas | 80h |
| 1 | VPC + RDS Multi-AZ | 3-4 semanas | 120h |
| 2 | Storage NFS → S3 | 2-3 semanas | 60h |
| 3 | ECS Fargate + ALB | 4-5 semanas | 160h |
| 4 | CI/CD + CloudWatch | 3-4 semanas | 80h |
| 5 | Backups + Hardening | 2 semanas | 40h |
| **Total** | | **16-21 semanas** | **~540h** |

**Custo AWS estimado (mensal, após migração completa):**
- ECS Fargate (2 tasks 0.5 vCPU / 1GB): ~$30-50/mês
- RDS PostgreSQL Multi-AZ (db.t3.medium): ~$90-120/mês
- S3 (estimativa 100GB): ~$3/mês
- ALB: ~$20/mês
- CloudWatch: ~$10-20/mês
- **Total estimado: ~$155-215/mês**

---

## O que funcionou

- O B-A-B forçou uma descrição técnica completa do estado atual, o que permitiu à IA identificar riscos específicos (ex: sessões Django, NFS POSIX vs S3)
- A sequência das fases foi tecnicamente correta: banco e storage antes da aplicação evita dependências circulares no cutover
- A IA entregou planos de rollback específicos por componente, não genéricos — cada rollback é acionável
- A estimativa de custo pós-migração foi incluída sem ser solicitada, um diferencial prático
- A sugestão de usar OIDC em vez de access key estático para GitHub Actions mostra conhecimento de boas práticas de segurança

## O que não funcionou

- A estimativa de esforço (~540h) não considera a curva de aprendizado da equipe se não tiver experiência AWS prévia
- Não foram mencionadas ferramentas de custo como AWS Cost Explorer ou budgets de alerta durante a migração
- A Fase 0 poderia ter incluído um checklist de compliance/LGPD antes de mover dados para cloud

## Ajustes que eu fiz

- Nenhuma reformulação foi necessária — a especificidade do Before (stack completa) e do After (cada componente de destino nomeado) gerou um plano diretamente acionável na primeira tentativa
- Em uso real, adicionaria ao Bridge: "A equipe tem 2 DevOps engineers com experiência básica em AWS. Inclua links para documentação oficial relevante em cada fase"

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O framework B-A-B demonstrou seu valor máximo neste caso de uso técnico complexo. A especificidade do Before (cada componente da stack atual) e do After (cada serviço AWS de destino) permitiu à IA gerar um plano de migração diretamente acionável, com riscos e rollbacks realistas. A ausência de necessidade de reformulação confirma que o contexto rico do B-A-B reduz drasticamente as iterações necessárias.

## O que eu faria diferente

Dividiria o Bridge em dois sub-prompts para cenários de alta complexidade: o primeiro gerando o plano de fases, o segundo pedindo à IA para atuar como revisor crítico do plano gerado e identificar o que ficou faltando (ex: compliance, treinamento da equipe, comunicação com os usuários internos durante a migração). O B-A-B é excelente para geração, mas um segundo prompt de revisão crítica eleva ainda mais a qualidade do output final.
