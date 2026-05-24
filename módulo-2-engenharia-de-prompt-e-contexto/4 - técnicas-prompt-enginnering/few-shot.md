# Few-Shot Prompting

---

## Prompt 1 — Escrita de artigo seguindo padrão de estilo

Prompt usado no Passo 1 da demo. Demonstra como o Few-Shot ensina o **estilo** de escrita ao modelo a partir de um único exemplo de referência. Sem o exemplo, o resultado sairia em um tom genérico e formal; com o exemplo, o modelo replica frases curtas, tom opinativo e analogias do cotidiano.

```
Quero que você escreva artigos seguindo o padrão de estilo do exemplo abaixo.

## Exemplo de referência:

Título: Por que Kubernetes não é pra todo mundo

Todo mundo fala de Kubernetes como se fosse a solução pra tudo. Spoiler: não é.

Se você tem 3 serviços rodando num VPS de 20 dólares, colocar Kubernetes no meio é como comprar um caminhão pra levar uma mochila. Funciona? Funciona. Faz sentido? Nem de longe.

O problema não é o Kubernetes — é achar que complexidade é sinal de maturidade. Às vezes o docker compose resolve. Às vezes um script bash resolve. E tá tudo bem.

Use Kubernetes quando a escala justificar. Quando o problema for real, não quando o ego pedir.

## Agora escreva um artigo neste mesmo estilo sobre: "Por que monitoramento não é só instalar Grafana"
```

---

## Prompt 2 — Mensagens de commit no padrão da equipe

Prompt usado na demo de commits semânticos. Com três exemplos no formato `tipo(escopo): descrição`, o modelo aprende o padrão completo de commit e replica para qualquer nova mudança. Sem o Few-Shot, cada commit sairia em um formato diferente.

```
Gere mensagens de commit seguindo o padrão da equipe. Siga os exemplos.

Exemplo 1:
Mudança: Adicionou endpoint de health check
Commit: feat(api): add health check endpoint with liveness and readiness probes

Exemplo 2:
Mudança: Corrigiu timeout na conexão com Redis
Commit: fix(cache): increase Redis connection timeout to 5s

Exemplo 3:
Mudança: Atualizou versão do Go de 1.20 para 1.21
Commit: chore(deps): bump Go version from 1.20 to 1.21

Agora gere:
Mudança: Adicionou retry com backoff exponencial nas chamadas ao serviço de pagamento
```

---

## Caderno de Experimentos

---

### Experimento 01 — Escrita de artigo seguindo padrão de estilo

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Teste de few-shot para replicar estilo de escrita técnica opinativa a partir de um único exemplo de referência, sem instrução explícita de tom ou estrutura.

#### Prompt usado

```
Quero que você escreva artigos seguindo o padrão de estilo do exemplo abaixo.

## Exemplo de referência:

Título: Por que Kubernetes não é pra todo mundo

Todo mundo fala de Kubernetes como se fosse a solução pra tudo. Spoiler: não é.

Se você tem 3 serviços rodando num VPS de 20 dólares, colocar Kubernetes no meio é como comprar um caminhão pra levar uma mochila. Funciona? Funciona. Faz sentido? Nem de longe.

O problema não é o Kubernetes — é achar que complexidade é sinal de maturidade. Às vezes o docker compose resolve. Às vezes um script bash resolve. E tá tudo bem.

Use Kubernetes quando a escala justificar. Quando o problema for real, não quando o ego pedir.

## Agora escreva um artigo neste mesmo estilo sobre: "Por que monitoramento não é só instalar Grafana"
```

#### Resposta da IA

Artigo com frases curtas, tom opinativo, analogias diretas e estrutura idêntica ao exemplo — sem precisar nomear o estilo explicitamente.

#### Output completo da IA

**Por que monitoramento não é só instalar Grafana**

Todo mundo fala de Grafana como se instalar o dashboard fosse resolver o problema de observabilidade. Spoiler: não é.

Se você só instalou o Grafana mas não tem métricas com semântica definida, alertas com SLOs reais e uma cultura de postmortem, você tem um painel bonito que ninguém abre quando o sistema cai. E quando cai, todo mundo olha pro Grafana como se ele fosse um oráculo.

O problema não é o Grafana — é achar que ferramenta substitui processo. Às vezes você precisa de um RED method definido antes de escolher o dashboard. Às vezes você precisa de alertas que disparam antes do cliente reclamar. E tá tudo bem começar por aí.

Use Grafana quando você já sabe o que quer medir. Quando o problema for de visibilidade, não de instalação.

#### O que funcionou

- Replicou frases curtas e tom direto do exemplo sem instrução explícita
- Analogia cotidiana no mesmo estilo ("painel bonito que ninguém abre")
- Estrutura de 4 parágrafos idêntica ao exemplo de referência
- "Spoiler:" como gancho de abertura preservado

#### O que não funcionou

- Conteúdo técnico menos específico que o exemplo — poderia citar RED method, SLOs ou USE method de forma mais concreta
- A frase final é um pouco genérica comparada ao nível de precisão do exemplo de referência

#### Ajustes que eu fiz

- Nenhum ajuste para esta demo. Em uso real, fornecer 2 exemplos de referência aumenta a consistência de tom em artigos mais longos.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Few-Shot transferiu o estilo do exemplo com precisão notável sem instrução explícita de "use frases curtas" ou "seja opinativo". Demonstra que o modelo aprende padrões de escrita por demonstração.

#### O que eu faria diferente

Fornecer 2 exemplos com temas diferentes para garantir que o estilo seja transferido independente do assunto, evitando que o modelo imite o conteúdo técnico do exemplo em vez do estilo.

---

### Experimento 02 — Mensagens de commit no padrão da equipe

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

#### Contexto

Teste de few-shot para padronizar mensagens de commit semântico a partir de 3 exemplos no formato `tipo(escopo): descrição`, replicando o padrão exato da equipe.

#### Prompt usado

```
Gere mensagens de commit seguindo o padrão da equipe. Siga os exemplos.

Exemplo 1:
Mudança: Adicionou endpoint de health check
Commit: feat(api): add health check endpoint with liveness and readiness probes

Exemplo 2:
Mudança: Corrigiu timeout na conexão com Redis
Commit: fix(cache): increase Redis connection timeout to 5s

Exemplo 3:
Mudança: Atualizou versão do Go de 1.20 para 1.21
Commit: chore(deps): bump Go version from 1.20 to 1.21

Agora gere:
Mudança: Adicionou retry com backoff exponencial nas chamadas ao serviço de pagamento
```

#### Resposta da IA

Mensagem de commit no formato correto, com tipo semântico adequado (`feat`), escopo preciso (`payment`) e descrição em inglês imperativo.

#### Output completo da IA

```
feat(payment): add exponential backoff retry mechanism for payment service calls
```

#### O que funcionou

- Tipo correto (`feat` — nova funcionalidade, não `fix`)
- Escopo inferido corretamente como `payment` a partir do contexto da mudança
- Descrição em inglês imperativo, como nos exemplos
- Comprimento adequado (dentro do limite de 72 caracteres)

#### O que não funcionou

- Sem variações — o modelo gerou apenas 1 opção. Em contexto real, pode ser útil pedir 2-3 alternativas
- Não incluiu body ou breaking change flag — pode ser necessário em commits mais complexos

#### Ajustes que eu fiz

- Nenhum ajuste necessário. Para uso em CI/CD com commit lint automatizado, adicionar: "retorne apenas a linha do commit, sem explicação".

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Few-Shot replicou o padrão com precisão: tipo correto, escopo inferido, inglês imperativo e comprimento adequado. Substitui completamente a necessidade de consultar as convenções da equipe em cada commit.

#### O que eu faria diferente

Adicionar um quarto exemplo com `feat!` (breaking change) para que o modelo também saiba quando usar o `!`. Isso torna o Few-Shot completo para todos os casos do Conventional Commits.
