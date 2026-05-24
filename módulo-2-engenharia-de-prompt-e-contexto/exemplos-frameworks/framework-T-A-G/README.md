## Prompt 1 — Plano de estudos com meta de certificação

**Cenário:** Plano de estudos para certificação AWS Solutions Architect com prazo de 3 meses. O Goal estabelece score mínimo e tempo disponível, fazendo o plano ser realista.

```markdown
# Task
Crie um plano de estudos semanal para a certificação AWS Solutions Architect Associate.

# Action
Organize por domínios do exame, distribua os tópicos em 12 semanas, inclua recursos gratuitos e pagos, e reserve as 2 últimas semanas para simulados e revisão.

# Goal
Objetivo: aprovação no exame com score mínimo de 750/1000, estudando no máximo 2 horas por dia durante a semana e 4 horas nos fins de semana.
```

## Prompt 2 — Análise de incidente com SLA de RCA

**Cenário:** RCA de incidente de timeout na API de pagamentos em ECS Fargate. O Goal de "até 2 horas, priorizando menor risco" muda toda a abordagem da análise.

```markdown
# Task
Analise o seguinte cenário de incidente: o serviço de API de pagamentos está retornando timeout (HTTP 504) para 30% das requisições desde as 14h. O serviço roda em ECS Fargate com 4 tasks, ALB na frente e banco RDS PostgreSQL.

# Action
Faça uma análise estruturada: liste as 5 causas mais prováveis em ordem de probabilidade, para cada causa indique o comando ou métrica AWS para validar, e proponha a ação corretiva imediata.

# Goal
Objetivo: produzir uma análise de causa raiz preliminar em formato de documento que permita ao time de plantão identificar e corrigir a causa raiz em até 2 horas, priorizando ações de menor risco primeiro.
```
