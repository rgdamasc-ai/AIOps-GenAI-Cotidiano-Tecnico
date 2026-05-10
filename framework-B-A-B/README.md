## Prompt 1 — Transição de carreira: sysadmin para Cloud Engineer

**Cenário:** Plano de transição de carreira para alguém saindo de sysadmin tradicional rumo a Cloud Engineer em 12 meses. Before descreve o estado atual com restrições reais e After o destino com marcos claros.

```markdown
# Before
Hoje trabalho como sysadmin em uma empresa de médio porte. Minha rotina é gerenciar servidores físicos, fazer deploys manuais via SSH, monitorar com Zabbix e apagar incêndios. Não tenho experiência com cloud, containers ou IaC. Meu inglês é intermediário.

# After
Quero estar trabalhando como Cloud Engineer em até 12 meses, com pelo menos uma certificação AWS, domínio básico de Terraform e Kubernetes, e capaz de participar de processos seletivos em empresas que usam cloud nativa.

# Bridge
Crie um plano de transição de carreira em fases mensais, considerando que posso dedicar 2 horas por dia durante a semana e 4 horas nos fins de semana. Inclua certificações recomendadas em ordem e projetos práticos para construir portfólio.
```

## Prompt 2 — Migração on-premises para AWS

**Cenário:** Plano de migração faseado de aplicação Python/Django on-premises para AWS (ECS Fargate, RDS, S3, GitHub Actions, CloudWatch, Terraform). Before descreve a stack atual completa e After especifica cada componente de destino.

```markdown
# Before
Temos uma aplicação web em Python/Django rodando em 2 VMs on-premises (NGINX + Gunicorn), banco PostgreSQL em uma VM dedicada, storage em NFS, e backups manuais semanais em fita. O monitoramento é básico (Nagios verificando uptime). Deploy é manual: o dev faz SSH, puxa o código do Git e reinicia o serviço. Não temos CI/CD. A aplicação atende ~500 usuários internos.

# After
Queremos a aplicação rodando na AWS com: containers em ECS Fargate, banco em RDS PostgreSQL Multi-AZ, storage em S3, pipeline CI/CD com GitHub Actions, monitoramento com CloudWatch e alertas no Slack, backups automáticos diários com retenção de 30 dias, e infraestrutura como código com Terraform.

# Bridge
Crie um plano de migração em fases, onde cada fase possa ser executada sem derrubar a aplicação em produção. Para cada fase inclua: o que será migrado, pré-requisitos, estimativa de esforço, riscos principais e plano de rollback.
```
