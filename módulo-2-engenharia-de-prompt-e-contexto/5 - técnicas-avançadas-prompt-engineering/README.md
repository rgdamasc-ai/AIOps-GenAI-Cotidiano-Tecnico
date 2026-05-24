# Técnicas Avançadas de Prompt Engineering

Este módulo reúne cinco técnicas avançadas de prompt engineering, com exemplos práticos voltados para profissionais de DevOps, SRE e Engenharia de Plataforma. Cada arquivo contém prompts prontos para uso, organizados em demos sequenciais.

---

## Índice de Técnicas

| Arquivo | Técnica | Quando usar |
|---|---|---|
| [prompt-chaining-diagramas.md](prompt-chaining-diagramas.md) | Prompt Chaining | Tarefas complexas em múltiplas etapas sequenciais |
| [least-to-most.md](least-to-most.md) | Least-to-Most | Problemas grandes que precisam ser decompostos antes de resolver |
| [chain-of-verification.md](chain-of-verification.md) | Chain of Verification | Quando precisão é crítica e o custo do erro é alto |
| [self-refine.md](self-refine.md) | Self-Refine | Iteração rápida com critérios de qualidade explícitos |
| [react.md](react.md) | ReAct | Quando o modelo precisa buscar informações externas para responder |

---

## Quando usar cada técnica

### Prompt Chaining
**Use quando:** a tarefa é grande demais para um único prompt e cada etapa depende do output da anterior. O Chaining força o modelo a focar em uma coisa de cada vez, reduzindo alucinações e aumentando a coerência entre as partes.

**Exemplos de aplicação:**
- Pipeline de resposta a incidentes (triagem → diagnóstico → remediação → post-mortem)
- Criação de conteúdo em múltiplas etapas (rascunho → revisão → formatação)
- Análise de código em fases (leitura → identificação de problemas → sugestão de correções)

**Arquivo:** [prompt-chaining-diagramas.md](prompt-chaining-diagramas.md)

---

### Least-to-Most
**Use quando:** o problema é complexo e você não sabe por onde começar, ou quando uma solução monolítica teria qualidade inferior. A técnica força o modelo a decompor antes de agir, respeitando dependências entre partes.

**Exemplos de aplicação:**
- Planejamento de infraestrutura cloud com múltiplos componentes interdependentes
- Criação de roadmaps técnicos com milestones e dependências
- Resolução de bugs em sistemas distribuídos (isolar componente → diagnosticar → corrigir)

**Arquivo:** [least-to-most.md](least-to-most.md)

---

### Chain of Verification (CoVe)
**Use quando:** o custo de uma resposta errada é alto — documentação técnica, configurações de segurança, análises que serão usadas para tomar decisões importantes. O isolamento entre o chat de geração e o chat de verificação é o diferencial que evita viés de confirmação.

**Exemplos de aplicação:**
- Validação de configurações Terraform, Kubernetes ou pipelines CI/CD
- Revisão de planos com restrições críticas (orçamento, compliance, segurança)
- Verificação de decisões arquiteturais antes de implementar

**Arquivo:** [chain-of-verification.md](chain-of-verification.md)

---

### Self-Refine
**Use quando:** você quer melhorar a qualidade de um output rapidamente, sem o custo de múltiplas janelas de contexto. É mais simples que o CoVe e suficiente quando o risco de erro é moderado. A chave é fornecer critérios de avaliação explícitos — sem eles, o modelo tende a se auto-elogiar.

**Exemplos de aplicação:**
- Refinamento de propostas, documentos e textos técnicos
- Melhoria de código com critérios de boas práticas definidos (segurança, performance, legibilidade)
- Iteração rápida em rascunhos onde uma segunda opinião humana não está disponível

**Arquivo:** [self-refine.md](self-refine.md)

---

### ReAct (Reason + Act)
**Use quando:** o modelo precisa de informações externas atualizadas para responder com precisão — dados que mudam com frequência, versões de software, CVEs, preços, eventos. Requer ferramenta de busca web habilitada. Sem ela, o ciclo degenera para Chain-of-Thought e o modelo alucina fatos.

**Exemplos de aplicação:**
- Análise de vulnerabilidades (CVEs) com versões e patches reais
- Pesquisa de soluções técnicas com documentação atualizada
- Comparação de opções (cloud providers, ferramentas, períodos) com dados reais

**Arquivo:** [react.md](react.md)

---

## Comparativo rápido

```
Técnica              | Isolamento | Custo   | Velocidade | Risco de viés
---------------------|------------|---------|------------|---------------
Prompt Chaining      | Não        | Médio   | Média      | Baixo
Least-to-Most        | Não        | Baixo   | Alta       | Baixo
Chain of Verification| Sim        | Alto    | Lenta      | Mínimo
Self-Refine          | Não        | Baixo   | Alta       | Moderado
ReAct                | Não        | Variável| Variável   | Baixo (com busca)
```

---

## Como usar os arquivos

Cada arquivo contém prompts prontos para copiar e colar em qualquer interface de chat com LLM (Claude, ChatGPT, Gemini, etc.). Os placeholders no formato `[colar output do Prompt N]` indicam onde inserir a resposta do passo anterior.

Para técnicas com múltiplos passos (Chaining, CoVe), execute os prompts na ordem indicada, na mesma sessão de chat, exceto quando o arquivo indicar explicitamente o uso de uma **janela separada**.

---

## Conteúdo dos arquivos

---

## Prompt Chaining - Diagrama

Os prompts abaixo formam duas cadeias independentes. Cada prompt depende do output do anterior — copie e execute em sequência na mesma sessão de chat. Os placeholders `[colar output do Prompt N]` indicam onde inserir a resposta anterior.

---

### Cadeia 1 — Post para LinkedIn em duas etapas

#### Passo 1 — Rascunho de conteúdo

Foco exclusivo na mensagem e no exemplo concreto. Formatação e engajamento são deliberadamente adiados para o próximo passo.

```
Sou um profissional de DevOps e quero escrever um post para LinkedIn
sobre uma lição importante que aprendi: automatizar sem entender o
processo manual primeiro leva a automações frágeis que quebram em
cenários não previstos.

Escreva um rascunho do post focando apenas no conteúdo e na mensagem.
Não se preocupe com hook, tom ou call-to-action por enquanto.
Inclua um exemplo concreto que ilustre a lição.
```

#### Passo 2 — Refinamento para engajamento

Recebe o rascunho do Passo 1 e aplica as regras de formatação de LinkedIn: hook, parágrafos curtos e pergunta final. Execute na mesma janela de contexto (o rascunho já está no histórico).

```
Pegue o rascunho abaixo e reescreva como um post de LinkedIn otimizado
para engajamento:

- Comece com um hook de 1-2 linhas que gere curiosidade
- Use tom conversacional e direto (como se estivesse falando com um
  colega)
- Quebre em parágrafos curtos (máximo 3 linhas cada)
- Termine com uma pergunta que convide comentários

Rascunho:
[colar o output do Prompt 1]
```

---

### Cadeia 2 — Pipeline de incidente em 4 etapas

Simula um fluxo completo de resposta a incidente: triagem → diagnóstico → remediação → documentação. Cada prompt recebe o output do anterior como entrada. Os dados brutos do incidente estão no Passo 1.

#### Passo 1 — Triagem dos dados

Organiza os dados brutos sem diagnosticar. O objetivo é estruturar fatos, padrões e correlações antes de qualquer hipótese.

```
Você é um SRE fazendo triagem de um incidente em produção.
Abaixo estão os dados brutos coletados do ambiente.

Sua tarefa: organizar esses dados num relatório de triagem
estruturado. Identifique padrões, correlações temporais e anomalias.
NÃO tente diagnosticar ainda. Apenas organize os fatos.

LOGS DA APLICAÇÃO (últimas 2h, filtrado por ERROR):
14:02:31 ERROR [req-a8f1] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:02:31 ERROR [req-a8f1] Handler /api/orders: 500 Internal Server Error
14:03:15 INFO  [req-b2c9] Handler /api/health: 200 OK
14:03:47 ERROR [req-d4e7] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:03:47 ERROR [req-d4e7] Handler /api/users: 500 Internal Server Error
14:04:02 INFO  [req-f1a3] Handler /api/orders: 200 OK (432ms)
14:04:55 ERROR [req-c7d2] DBPool: Pool exhausted, waiting for available connection... timeout after 5000ms
14:04:55 ERROR [req-c7d2] Handler /api/products: 500 Internal Server Error
14:05:11 INFO  [req-e9b4] Handler /api/users: 200 OK (387ms)
14:06:03 ERROR [req-h3j8] DBPool: Connection attempt failed - ECONNREFUSED 10.0.3.42:5432
14:08:22 ERROR [req-k5m1] DBPool: Pool exhausted, waiting for available connection... timeout after 5000ms

MÉTRICAS DO RDS (CloudWatch, últimas 2h):
- DatabaseConnections: 98-100 (limite: max_connections=100)
- CPUUtilization: 92-96%
- FreeableMemory: 1.2GB de 8GB
- ReadLatency: 45ms (normal: 5-8ms)
- WriteLatency: 62ms (normal: 8-12ms)

STATUS DOS PODS (kubectl):
NAME                        READY   STATUS    RESTARTS   AGE
api-server-6d4f8b7c9-abc12  1/1     Running   0          18h
api-server-6d4f8b7c9-def34  1/1     Running   0          18h
api-server-6d4f8b7c9-ghi56  1/1     Running   0          18h

TRECHO DO ÚLTIMO DEPLOY (diff do values.yaml, ontem 18h):
  db:
    pool:
-     size: 10
+     size: 50
    host: prod-db.abc123.us-east-1.rds.amazonaws.com

AMBIENTE:
- App: Node.js, EKS com 3 réplicas
- Banco: PostgreSQL RDS (db.r5.large, max_connections=100)
- Início dos erros: 14h hoje. Erros afetam ~30% das requisições.

Gere o relatório de triagem no formato:
1. Resumo da situação (3 linhas)
2. Padrões identificados nos logs (com evidências)
3. Anomalias nas métricas (comparando com valores normais)
4. Mudanças recentes relevantes
5. Correlações entre os dados
```

#### Passo 2 — Diagnóstico da causa raiz

Recebe o relatório de triagem e constrói a cadeia causal completa com evidências e hipóteses descartadas.

```
Com base no relatório de triagem abaixo, diagnostique a causa raiz
deste incidente.

[colar output do Prompt 1]

Apresente:
1. Causa raiz principal (com a cadeia causal completa: o que mudou →
   o que isso causou → por que gerou o sintoma observado)
2. Evidências dos dados que sustentam o diagnóstico
3. Hipóteses alternativas consideradas e por que foram descartadas
```

#### Passo 3 — Plano de remediação

Recebe o diagnóstico e propõe ações em duas fases: mitigação imediata e correção definitiva, ambas com comandos específicos e critérios de validação.

```
Com base no diagnóstico abaixo, proponha um plano de remediação.

[colar output do Prompt 2]

Contexto técnico:
- App: Node.js em EKS, 3 réplicas
- Banco: PostgreSQL RDS db.r5.large, max_connections=100
- Pool size atual: 50 por réplica (configurado em values.yaml)
- Pool size anterior: 10 por réplica

Proponha um plano em duas fases:
1. Ação imediata para parar o sangramento (mitigação)
2. Correção definitiva para evitar recorrência

Para cada ação, indique: o que fazer, comando ou configuração
específica, risco envolvido e como validar que funcionou.
```

#### Passo 4 — Post-mortem

Consolida triagem, diagnóstico e remediação em um documento formal de post-mortem pronto para compartilhar com a equipe.

```
Com base nas informações abaixo, gere a documentação post-mortem
deste incidente.

Relatório de triagem:
[output do Prompt 1]

Diagnóstico:
[output do Prompt 2]

Remediação aplicada:
[output do Prompt 3]

Gere o post-mortem no formato:
- Resumo do incidente (3 linhas)
- Timeline (desde o deploy até a resolução)
- Causa raiz
- Impacto (usuários afetados, duração, endpoints impactados)
- Ações tomadas (imediatas e definitivas)
- Action items para prevenir recorrência
```

---

## Least to Most

Os prompts abaixo demonstram duas variações da técnica: decomposição por subproblemas com dependências explícitas e decomposição por milestones. Execute cada prompt de resolução na mesma sessão de chat, em continuidade ao prompt de decomposição anterior.

---

### Decomposição de infraestrutura AWS em subproblemas

Primeiro prompt da Demo 1. O modelo retorna apenas a lista de subproblemas agrupados por dependência, sem implementar nada. O "Retorne apenas a lista" é intencional: evita que o modelo tente resolver tudo de uma vez.

```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Decomponha a criação dessa infraestrutura em subproblemas agrupados
por dependência. Para cada subproblema, liste o que ele envolve e
de quais subproblemas anteriores ele depende. Ordene do mais
fundamental ao mais complexo.
Retorne apenas a lista.
```

---

### Resolução do Subproblema 1 (Networking/VPC)

Enviado na sequência da decomposição, na mesma janela de contexto. O contexto acumulado substitui a necessidade de repetir toda a descrição da infra.

```
Estou criando a infraestrutura na AWS para uma aplicação web: API em
containers (EKS), PostgreSQL (RDS), Redis (ElastiCache), API Gateway.
Multi-AZ, com ambientes staging e produção separados.

Resolva o subproblema 1.
```

---

### Resolução do próximo subproblema pendente

Variação minimalista para continuar a cadeia após resolver o Subproblema 1. O modelo usa o contexto da conversa para identificar qual é o próximo subproblema ainda não resolvido.

```
Resolva o próximo subproblema pendente.
```

---

### Decomposição de infraestrutura AWS em milestones

Variação da Demo 1 com formato de milestones e sub-tarefas ordenados por dependência. Gera uma estrutura mais próxima de um plano de projeto do que de uma lista técnica de recursos.

```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Crie uma lista de milestones com tarefas e sub-tarefas para a criação
dessa infraestrutura. Organize por ordem de execução, considerando
dependências entre os milestones. Retorne apenas a lista.
```

---

### Execução progressiva de milestone

Prompt de execução da Demo de milestones. Enviado na sequência da decomposição anterior para que o modelo expanda e implemente um milestone por vez.

```
Execute a próxima tarefa.
```

---

## Chain of Verification

Os prompts abaixo demonstram o ciclo completo de Chain of Verification (CoVe) em duas demos: planejamento de churrasco (exemplo do cotidiano) e validação de configuração Terraform (cenário técnico). Cada ciclo tem 4 passos: gerar resposta inicial → gerar perguntas de verificação → responder em janela isolada → revisar e corrigir.

**Importante:** os Passos 1, 2 e 4 rodam na mesma janela (Chat 1). O Passo 3 roda em uma janela nova (Chat 2), sem acesso à resposta original. Esse isolamento é o que evita viés de confirmação.

---

### Demo 1 — Planejamento de Churrasco (Passo 1: plano inicial)

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

### Demo 1 — Planejamento de Churrasco (Passo 2: perguntas de verificação)

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

### Demo 1 — Planejamento de Churrasco (Passo 3: responder em contexto isolado)

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

### Demo 1 — Planejamento de Churrasco (Passo 4: revisão e correção)

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

### Demo 2 — Validação de Terraform (Passo 1: gerar configuração)

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

### Demo 2 — Validação de Terraform (Passo 2: perguntas de verificação técnicas)

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

### Demo 2 — Validação de Terraform (Passo 3: responder em contexto isolado)

Chat 2 (janela nova, sem o Terraform original). O modelo responde sobre boas práticas AWS — não sobre o código gerado. Esse é o ponto de não-viés.

```
Responda as seguintes perguntas sobre boas práticas AWS para
uma aplicação web com VPC, RDS PostgreSQL, EKS e ALB:

[colar APENAS as perguntas do Chat 1]

Para cada pergunta responda SIM ou NÃO e uma frase curta
justificando. Máximo 2 linhas por resposta.
```

---

### Demo 2 — Validação de Terraform (Passo 4: revisão e correção do código)

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

## Self Refine

Os prompts abaixo demonstram o ciclo Self-Refine completo em duas demos: proposta de palestra (CFP) e Dockerfile. Cada ciclo tem 3 passos — gerar → criticar → melhorar — executados na mesma janela de contexto.

A diferença para o Chain of Verification é que aqui o modelo critica e melhora o próprio output sem isolamento. É mais barato e rápido, mas suscetível a viés de confirmação. Use quando o custo do erro é baixo e a velocidade importa.

---

### Demo 1 — Proposta de palestra (Passo 1: gerar a proposta)

Pedido inicial de uma CFP. Estrutura mínima: título, abstract e 3-5 tópicos. Sem critérios de qualidade ainda — o objetivo é ter uma versão 1 para criticar.

```
Escreva uma proposta de palestra (CFP) para submeter num evento
de tecnologia. O tema é "Como IA generativa está mudando o dia
a dia de times de engenharia de plataforma". O formato é talk de
30 minutos. O público são engenheiros de software e DevOps com
nível intermediário a avançado. A proposta precisa ter: título,
abstract e 3 a 5 tópicos que serão abordados.
```

---

### Demo 1 — Proposta de palestra (Passo 2: autocrítica com critérios)

Mesmo chat. O modelo agora avalia o que ele mesmo escreveu, usando 5 critérios com nota de 1 a 5. Sem os critérios explícitos, a autocrítica vira elogio genérico.

```
Critique a proposta de palestra que você gerou. Avalie com base
nos seguintes critérios:

1. Gancho: o título e o início do abstract capturam atenção ou são genéricos?
2. Clareza: quem ler entende exatamente o que vai aprender assistindo?
3. Originalidade: a proposta se diferencia de dezenas de outras sobre IA?
4. Público-alvo: o nível técnico tá calibrado pra engenheiros intermediários/avançados?
5. Formato: os tópicos cabem em 30 minutos sem virar superficial?

Para cada critério, dê uma nota de 1 a 5 e justifique.
Retorne em formato markdown.
```

---

### Demo 1 — Proposta de palestra (Passo 3: versão melhorada)

Terceiro passo do ciclo. Pede a v2 corrigindo apenas o que ficou abaixo de 4 — isso evita mudanças cosméticas em pontos que já estavam bons.

```
Com base na sua autocrítica, gere uma versão melhorada da proposta.
Corrija todos os pontos onde a nota foi menor que 4.
Retorne em formato markdown.
```

---

### Demo 2 — Dockerfile (Passo 1: gerar o Dockerfile)

Cenário técnico. Pedido propositalmente vago em segurança/boas práticas — o ciclo Self-Refine vai expor isso depois.

```
Gere um Dockerfile para uma API Node.js com Express.
A aplicação usa npm, tem um arquivo package.json e o
entrypoint é server.js. Gere pronto para build.
gere apenas o dockerfile
```

---

### Demo 2 — Dockerfile (Passo 2: autocrítica com critérios técnicos)

Mesmo chat. 5 critérios objetivos focados em produção de containers. Os critérios mudam por domínio — aqui são técnicos; na demo da CFP eram editoriais.

```
Critique o Dockerfile que você gerou. Avalie com base
nos seguintes critérios:

1. Segurança: roda como root? Tem usuário não-privilegiado?
2. Tamanho da imagem: a imagem base é a mais enxuta possível?
3. Cache de layers: a ordem dos comandos aproveita o cache do Docker?
4. Multi-stage: separa build de runtime?
5. Boas práticas: tem .dockerignore? Usa COPY em vez de ADD?

Para cada critério, dê uma nota de 1 a 5 e explique o que melhorar.
Retorne apenas a avaliação
```

---

### Demo 2 — Dockerfile (Passo 3: versão melhorada)

Fechamento do ciclo. Mesma instrução da Demo 1 — só corrigir o que ficou abaixo de 4.

```
Com base na sua autocrítica, gere uma versão melhorada do
Dockerfile. Corrija todos os pontos onde a nota foi menor que 4.
```

---

## ReAct

Os dois prompts abaixo demonstram o ciclo Thought → Action → Observation do ReAct em dois cenários: um do cotidiano (planejamento de viagem) e outro técnico (análise de impacto de CVEs). Ambos exigem ferramenta de busca web habilitada — sem isso, o modelo não consegue executar "Action" e o ciclo degenera para CoT.

A instrução-chave em ambos é "não invente nada, pesquise tudo". Sem essa instrução explícita, o modelo tende a alucinar dados em vez de buscar.

---

### Cenário do cotidiano — Planejamento de viagem para Natal/RN

Cenário acessível para introduzir o padrão. O modelo precisa pesquisar clima, preços, lotação e eventos em três meses diferentes, comparar e recomendar. A obrigação de citar fonte para cada dado força o ciclo de Action → Observation.

```
Preciso planejar uma viagem de 5 dias para Natal/RN.
Tenho três opções de período: janeiro, junho ou setembro.

Preciso que você:
1. Pesquise e compare clima, preços, lotação e eventos
   em cada um dos três meses
2. Recomende o melhor mês com base nos dados encontrados
3. Monte um roteiro de 5 dias com atrações reais

Importante: não invente nenhuma informação. Pesquise
tudo antes de responder. Para cada dado, me diga
de onde veio.
```

---

### Cenário técnico — Análise de impacto consolidada de CVEs

Cenário próximo ao trabalho de SRE/Segurança. Recebe três CVEs detectadas pelo Trivy e pede análise individual + priorização contextualizada (Kubernetes restricted, CI/CD com git, base Debian). A contextualização do ambiente é o que diferencia uma análise útil de um dump genérico de CVSS.

```
Nosso scanner de segurança (Trivy) detectou 3 CVEs na imagem
base de um container em produção. Preciso que você investigue
cada uma e monte uma análise de impacto consolidada.

CVEs detectadas:
1. CVE-2024-21626 — componente: runc (container runtime)
   Versão instalada: 1.1.10
2. CVE-2024-32002 — componente: git
   Versão instalada: 2.43.0
3. CVE-2024-3094 — componente: xz-utils/liblzma
   Versão instalada: 5.4.5

Para cada CVE, investigue:
- O que explora e qual a severidade (CVSS)
- Quais versões são afetadas
- Se a versão que usamos está vulnerável
- Qual o patch disponível

Ao final, monte uma análise consolidada com priorização
de remediação considerando que:
- Nossos containers rodam em Kubernetes com Pod Security
  Standards no modo restricted
- A imagem é usada em pipelines CI/CD que fazem git clone
- A imagem é baseada em Debian stable

Pesquise tudo. Não invente nenhuma informação de
severidade, versão ou patch.
Retorne os dados em markdown
```
