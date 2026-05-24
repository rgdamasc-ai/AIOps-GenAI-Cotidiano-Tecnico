## Prompt 1 — Role: pergunta sem persona definida

**Cenário:** Pergunta ambígua sobre "manga" sem role definido. Demonstra como o modelo mistura significados (fruta, peça de roupa, mangá) quando não sabe qual perspectiva adotar.

```
Me fale sobre manga
```

## Prompt 2 — Role de alfaiate: pergunta sobre manga

**Cenário:** Mesma pergunta com role de alfaiate. O modelo passa a falar sobre tipos de manga em camisas (longa, curta, raglan), corte, caimento e tecido.

```
Você é um alfaiate especializado em camisas sob medida.

Me fale sobre manga.
```

## Prompt 3 — Role de hortifruti: pergunta sobre manga

**Cenário:** Mesma pergunta com role de especialista em frutas. O modelo passa a falar sobre variedades (Tommy, Palmer, Espada), safra e armazenamento.

```
Você é um especialista em frutas tropicais que trabalha num hortifruti.

Me fale sobre manga.
```

## Prompt 4 — Role de otaku: pergunta sobre manga

**Cenário:** Mesma pergunta com role de especialista em cultura pop japonesa. O modelo passa a falar sobre história do mangá, gêneros (shonen, seinen, shojo) e autores clássicos.

```
Você é um especialista em cultura pop japonesa e dono de uma loja de mangás.

Me fale sobre manga.
```

## Prompt 5 — Contexto: pergunta de latência sem contexto

**Cenário:** Pergunta genérica sobre como reduzir latência sem contexto da aplicação. Resultado é uma lista vaga que serve para qualquer cenário.

```
Como reduzir a latência da minha aplicação?
```

## Prompt 6 — Contexto: pergunta de latência com contexto detalhado

**Cenário:** Mesma pergunta enriquecida com contexto técnico (stack, sintoma, métricas). O modelo direciona a investigação para causas prováveis baseadas no cenário real.

```
Temos uma API em Node.js rodando no ECS Fargate, com banco PostgreSQL no RDS.
A latência do endpoint /pedidos subiu de 200ms para 1.2s após o deploy de sexta.
O RDS está em 85% de CPU. Não houve mudança de infra, apenas código.

Como reduzir a latência?
```

## Prompt 7 — Instrução vaga sobre Kubernetes

**Cenário:** Pedido genérico sem verbo claro. O modelo precisa adivinhar se deve explicar, diagnosticar ou criar documentação.

```
Me ajuda com Kubernetes.
```

## Prompt 8 — Instrução específica: explicar diferenças entre recursos

**Cenário:** Verbo "explique" deixa claro o tipo de entrega esperada. Resultado é uma explicação focada nos recursos pedidos.

```
Explique a diferença entre Deployment, StatefulSet e DaemonSet no Kubernetes.
```

## Prompt 9 — Instrução específica: comparar serviços AWS

**Cenário:** Verbo "compare" indica que o modelo deve produzir uma análise comparativa com trade-offs focada num cenário específico.

```
Compare ECS Fargate e EKS para rodar microsserviços de uma startup com 3 desenvolvedores.
```

## Prompt 10 — Instrução específica: recomendar estratégia de deploy

**Cenário:** Verbo "recomende" pede uma sugestão estruturada (passo a passo, pré-requisitos, fluxo e rollback).

```
Recomende uma estratégia de deploy blue-green para uma API que não pode ter downtime.
```

## Prompt 11 — Output sem formato definido

**Cenário:** Pedido para listar serviços AWS sem formato específico. O resultado vem em texto corrido misturando explicações e exemplos.

```
Liste os serviços AWS para hospedar containers.
```

## Prompt 12 — Output em tabela comparativa Markdown

**Cenário:** Mesmo pedido com formato definido (tabela Markdown), restrições de quantidade e ordem. Resultado pronto para colar em documentação.

```
Liste os serviços AWS para hospedar containers.

Formato: tabela Markdown com colunas Serviço | Tipo (serverless/managed) | Caso de uso ideal | Custo relativo.
Máximo 5 serviços. Ordene do mais simples ao mais complexo.
```

## Prompt 13 — Output em JSON estruturado

**Cenário:** Mesmo pedido com formato JSON e filtro (apenas serverless). Saída pronta para alimentar outro sistema.

```
Liste os serviços AWS para hospedar containers.

Formato: array JSON com objetos contendo campos "service", "type", "use_case".
Apenas serviços serverless.
```

## Prompt 14 — Output com JSON schema explícito

**Cenário:** Pedido para listar alertas com schema JSON formalizado funcionando como contrato. Saída tipada e consistente, pronta para integração com PagerDuty, Slack ou dashboards.

```
Liste os 5 alertas mais comuns em clusters Kubernetes.

Formato: array JSON seguindo este schema:
{
  "alert_name": "string",
  "severity": "critical | warning | info",
  "component": "string",
  "typical_cause": "string",
  "first_action": "string (comando kubectl)"
}
Retorne apenas o JSON, sem explicações.
```

## Prompt 15 — Demo desestruturada: planejamento de viagem

**Cenário:** Pedido genérico de planejamento de viagem sem nenhum dos 4 componentes. Resultado é uma lista de dicas vagas.

```
Me ajuda a planejar uma viagem
```

## Prompt 16 — Demo estruturada: planejamento de viagem com 4 componentes

**Cenário:** Mesmo pedido reestruturado com Role, Contexto, Instrução e Output. Resultado é um roteiro personalizado dentro do orçamento e restrições.

```
# Role
Você é um agente de viagens especializado em roteiros econômicos pela América do Sul.

# Contexto
Casal sem filhos, orçamento de R$ 5.000 total, disponibilidade de 7 dias em julho,
saindo de São Paulo. Preferem natureza e trilhas, não curtem balada.

# Instrução
Monte um roteiro dia a dia com destino, atividades e estimativa de custo.

# Output
Formato: tabela Markdown com colunas Dia | Destino | Atividade | Custo estimado.
Inclua totais ao final. Apenas opções dentro do orçamento.
```

## Prompt 17 — Demo desestruturada: problema de OOMKills no Kubernetes

**Cenário:** Pedido vago de ajuda com problema de memória no Kubernetes. Resultado é uma lista genérica de possibilidades sem comandos reais.

```
Me ajuda a resolver um problema de memória no Kubernetes
```

## Prompt 18 — Demo estruturada: runbook de OOMKills com 4 componentes

**Cenário:** Mesmo problema reestruturado com Role, Contexto, Instrução e Output. Resultado é um runbook acionável com comandos kubectl reais.

```
# Role
Você é um SRE senior com 10 anos de experiência em Kubernetes e AWS.

# Contexto
Temos um cluster EKS com 50 nodes e estamos tendo OOMKills frequentes nos pods de API.

# Instrução
Crie um runbook de troubleshooting para diagnosticar e resolver OOMKills.

# Output
Formato: tabela Markdown com colunas Passo | Comando | O que verificar | Ação.
Máximo 10 passos. Use apenas comandos kubectl reais, não genéricos.
```
