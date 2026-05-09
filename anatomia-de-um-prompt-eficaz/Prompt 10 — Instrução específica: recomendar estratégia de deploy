# Experimento 10 — Instrução específica: recomendar estratégia de deploy

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Terceiro cenário do grupo "Instrução" — verbo "Recomende" aplicado a uma estratégia de deploy blue-green com restrição explícita (zero downtime). Demonstra como o verbo "Recomende" sinaliza ao modelo que a entrega deve ser prescritiva e estruturada com pré-requisitos, fluxo passo a passo e rollback.

## Prompt usado

```
Recomende uma estratégia de deploy blue-green para uma API que não pode ter downtime.
```

## Resposta da IA

O modelo entregou uma recomendação estruturada em quatro seções: Pré-requisitos (LB com weighted routing, health checks, migrações backward-compatible, automação de rollback), Fluxo de 7 passos com percentuais de tráfego progressivos (10% → 30% → 50% → 100%), Rollback com SLA de 30 segundos e Atenção crítica sobre compatibilidade de banco de dados durante coexistência Blue/Green.

## Output completo da IA

**Estratégia Blue-Green para API sem downtime**

**Pré-requisitos:**
- Load balancer com suporte a weighted routing (ALB, NGINX, Traefik)
- Health checks configurados no novo ambiente antes de receber tráfego
- Banco de dados compatível com ambas as versões simultaneamente (migrações backward-compatible)
- Automação de rollback (script ou pipeline CI/CD)

**Fluxo recomendado:**

1. **Provisione o ambiente Green** com a nova versão enquanto Blue serve 100% do tráfego
2. **Execute smoke tests** no Green apontando diretamente para ele (sem passar pelo LB de produção)
3. **Direcione 10% do tráfego** para Green e monitore por 5-10 minutos (error rate, latência, logs)
4. **Aumente gradualmente:** 10% → 30% → 50% → 100% com janelas de observação entre cada passo
5. **Confirme estabilidade** por 15-30 minutos com 100% no Green
6. **Mantenha Blue de pé** por pelo menos 1 hora como contingência de rollback imediato
7. **Desprovisione Blue** após confirmação completa

**Rollback:**
Se qualquer métrica sair do threshold em qualquer passo, redirecione 100% para Blue imediatamente. O rollback deve ser executável em menos de 30 segundos via mudança no LB.

**Atenção crítica — banco de dados:**
Durante a coexistência Blue/Green, ambas as versões escrevem no mesmo banco. Garanta que:
- Migrações adicionam colunas, nunca renomeiam ou deletam
- O código Blue seja capaz de ignorar colunas novas (backward-compatible reads)
- Só remova colunas antigas num deploy posterior, após Blue ser descomissionado

## O que funcionou

- Verbo "Recomende" produziu uma entrega prescritiva com pré-requisitos, fluxo e rollback — sem solicitar explicitamente nenhuma dessas seções
- Alerta crítico sobre banco de dados (migrações backward-compatible, coexistência de versões) é o ponto mais ignorado na prática e foi destacado corretamente
- SLA de rollback quantificado (30 segundos) torna a recomendação auditável
- Percentuais progressivos de tráfego (10%→30%→50%→100%) são uma prática real, não genérica

## O que não funcionou

- Não mencionou ferramentas específicas de deployment (CodeDeploy, Argo Rollouts, Spinnaker) que implementam esse padrão nativamente
- Poderia ter incluído um exemplo de configuração de weighted routing no ALB

## Ajustes que eu fiz

- Nenhum — o verbo "Recomende" + a restrição de negócio (zero downtime) foram suficientes para estruturar a resposta

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O verbo "Recomende" produziu espontaneamente pré-requisitos, fluxo, rollback e alerta de risco — estrutura completa de uma recomendação de engenharia. Fecha o trio de verbos (Explique, Compare, Recomende) mostrando como cada um produz um tipo de entrega radicalmente diferente para o mesmo domínio.

## O que eu faria diferente

Adicionar a stack de infraestrutura atual (ex: "usando ALB + ECS Fargate") para que o modelo inclua comandos e configurações específicas da ferramenta em vez de uma recomendação agnóstica de tecnologia.
