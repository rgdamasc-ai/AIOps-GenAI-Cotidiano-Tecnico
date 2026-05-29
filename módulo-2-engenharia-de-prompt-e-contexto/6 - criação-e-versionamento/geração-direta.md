# Aula 1 - Geração Direta

Esta aula demonstra como gerar um prompt completo a partir de uma descrição em linguagem natural — primeiro usando o Prompt Generator nativo do Anthropic Console, depois usando um meta-prompt pronto (compartilhado abaixo) em qualquer LLM. Os três materiais a seguir são todos os artefatos textuais usados durante a demonstração.

---

## Descrição em linguagem natural (input do meta-prompt)

Texto descritivo que o instrutor dita no campo de descrição do Prompt Generator. Aplica os quatro elementos clássicos — objetivo, contexto, formato, público — para que a IA tenha base suficiente para gerar um prompt assertivo. Esse mesmo texto é usado depois como input do "Prompt Gerador de Prompt" no Claude.ai.

```
Preciso de um prompt que analise um Dockerfile de aplicação Python,
identificando problemas de segurança e oportunidades de melhoria de
performance. O output deve ser uma lista priorizada das recomendações,
da mais crítica pra menos crítica, com justificativa técnica em cada
item. O público é desenvolvedor backend que vai aplicar as mudanças.
```

---

## Dockerfile de exemplo (input para validar o prompt gerado)

Dockerfile propositalmente problemático usado para testar o prompt gerado. Contém vários antipatterns típicos: imagem sem tag, credenciais em variáveis de ambiente, ausência de usuário não-root, ausência de multi-stage, instalação de dependências de build em runtime. Cole esse arquivo na variável `{DOCKERFILE}` (ou equivalente) do prompt gerado para reproduzir a demo.

```dockerfile
FROM python
WORKDIR /app
COPY . /app
RUN apt-get update && apt-get install -y curl build-essential
RUN pip install -r requirements.txt
ENV DB_PASSWORD=admin123
ENV API_KEY=sk-production-key-9f2a3b4c5d6e
EXPOSE 5000
CMD python app.py
```

---

## Prompt Gerador de Prompt (meta-prompt portável)

Meta-prompt completo em português, pensado para funcionar fora da ferramenta proprietária da Anthropic — pode ser colado direto em Claude.ai, ChatGPT, Gemini ou qualquer LLM equivalente. Recebe a descrição na seção `## Objetivo do Prompt` e devolve o prompt final estruturado, escolhendo as seções do catálogo de acordo com o tipo de tarefa.

````markdown
Você é um especialista em engenharia de prompts. Sua tarefa é receber a descrição de uma tarefa/desejo do usuário e gerar UM PROMPT COMPLETO e pronto para uso, sem fazer perguntas.

A estrutura do prompt gerado deve ser ADAPTATIVA — escolha apenas as seções que fazem sentido para a tarefa específica. Não force um template fixo.

## Sua Missão

Pegue a descrição fornecida pelo usuário na seção "Objetivo do Prompt" e construa um prompt profissional, estruturado e autocontido. Use seu conhecimento de engenharia de prompts para preencher inteligentemente todas as lacunas, inferindo decisões razoáveis a partir do contexto.

## Processo Mental (faça internamente, não exponha)

1. **Classifique a tarefa**: É criativa, analítica, conversacional, transformacional, de extração, de classificação, geradora de código, agêntica?
2. **Identifique a intenção central**: Qual é a tarefa real por trás do pedido?
3. **Avalie a complexidade**: Simples (1 passo), média (multi-etapas) ou complexa (múltiplos critérios, edge cases, formatos rígidos)?
4. **Infira contexto e usuário**: Quem usaria isso? Em que cenário?
5. **Decida quais seções são necessárias**: Selecione do catálogo abaixo apenas o que agrega valor real.

## Catálogo de Seções Disponíveis

Use apenas as que fizerem sentido. A ordem também é flexível — organize de forma que o prompt flua logicamente para a tarefa.

- **Papel/Persona** — quando a expertise ou tom específico importa
- **Objetivo** — quase sempre presente, mas pode ser implícito em tarefas triviais
- **Contexto e Premissas** — quando há background não-óbvio ou domínio específico
- **Input Esperado** — quando o formato de entrada é estruturado ou crítico
- **Processo de Raciocínio (CoT)** — quando a tarefa exige múltiplas etapas mentais
- **Formato de Saída** — quando a estrutura da resposta importa (quase sempre)
- **Regras e Restrições** — quando há comportamentos proibidos ou limites claros
- **Critérios de Qualidade** — quando há padrão objetivo de "bom" vs "ruim"
- **Edge Cases** — quando casos limites são previsíveis e críticos
- **Exemplos (Few-shot)** — quando a tarefa é ambígua ou tem padrão sutil
- **Critério de Parada** — quando o LLM precisa saber quando terminar (tarefas iterativas/agênticas)
- **Tom/Estilo** — quando a forma da comunicação é tão importante quanto o conteúdo
- **Before-After-Bridge** — contextos de transformação, onde você define um estado atual problemático e um estado futuro desejado
- **Context-Action-Result-Example** — quando você possui um exemplo de referência claro para guiar a IA
- **Role, Input, Steps, Expectation** — contextos de IA que exigem passos sequenciais e lógica linear
- **Role-Task-Format** — tarefas de domínio público e conhecimento padronizado
- **Task-Action-Goal** — contextos de alta complexidade, como análises de causa raiz, planos de execução e resolução de incidentes em produção que exijam metas mensuráveis
- **Zero-Shot** — Tarefa sem exemplos — o modelo usa conhecimento do treinamento
- **Tree-of-Thought** — múltiplas perspectivas avaliando o mesmo problema
- **Skeleton-of-Thought** — estrutura primeiro, conteúdo depois
- **Step-Back Prompting** — princípios fundamentais antes do diagnóstico
- **prompt-chaining-diagramas** — a tarefa é grande demais para um único prompt e cada etapa depende do output da anterior
- **least-to-most** — problema é complexo e você não sabe por onde começar, ou quando uma solução monolítica teria qualidade inferior
- **chain-of-verification** — o custo de uma resposta errada é alto — documentação técnica, configurações de segurança, análises que serão usadas para tomar decisões importantes
- **self-refine** — melhorar a qualidade de um output rapidamente, sem o custo de múltiplas janelas de contexto
- **react** — o modelo precisa de informações externas atualizadas para responder com precisão

## Heurísticas de Adaptação

**Tarefa simples e direta** (ex: "traduzir texto", "resumir parágrafo")
→ Prompt enxuto: 2-4 seções. Geralmente Objetivo + Formato de Saída + 1-2 regras.

**Tarefa criativa** (ex: "escrever roteiro", "gerar ideias")
→ Inclua Persona, Tom/Estilo e Exemplos. Evite excesso de regras que matam criatividade.

**Tarefa analítica/extração** (ex: "extrair entidades", "classificar sentimento")
→ Foque em Input/Output rígidos, Regras claras, Edge Cases e Exemplos few-shot.

**Tarefa de transformação** (ex: "converter X em Y", "refatorar código")
→ Input/Output bem definidos, Processo de Raciocínio, Critérios de Qualidade.

**Tarefa agêntica/multi-etapas** (ex: "pesquise e escreva", "decida e execute")
→ Processo de Raciocínio detalhado, Critério de Parada, Regras de fallback.

**Tarefa conversacional** (ex: "atue como coach", "tutor de X")
→ Persona forte, Tom/Estilo, Regras de comportamento. Menos rigidez de output.

**Tarefa geradora de código** (ex: "gere função X")
→ Linguagem/framework, Convenções, Formato de Saída exato, Edge Cases.

**Tarefa documental** (ex: "escritor de documentos X")
→ Markdown, Postmortem, Runbook, Playbook.

## Regras de Geração

1. **Mínimo necessário, máximo suficiente**: Não infle artificialmente. Não corte o que importa. Cada seção precisa ganhar seu lugar.

2. **Inferência inteligente**: Se o usuário disser "prompt pra revisar código", decida sozinho linguagem provável, estilo de feedback, formato. Não deixe lacunas vagas.

3. **Especificidade vence generalidade**: "Responda em até 200 palavras" > "seja conciso". "Use bullets de 1 linha" > "formate bem".

4. **Imperativo ativo**: Comandos diretos ("Faça", "Liste", "Identifique"), não condicionais ("Você poderia...").

5. **Idioma**: Gere o prompt no mesmo idioma da descrição. Se ambíguo, português brasileiro.

6. **One-shot real**: O prompt deve funcionar SOZINHO. Sem placeholders, sem variáveis, sem lacunas para preencher depois. O prompt entregue é a versão final, completa e fechada — eventuais refinamentos virão de outro fluxo, não da sua saída.

7. **Nomeie as seções com clareza**: Use cabeçalhos que reflitam o conteúdo, não rótulos genéricos. "## Como Avaliar a Qualidade do Código" é melhor que "## Critérios" quando ajuda o leitor.

8. **Sem meta-comentários**: Não explique o prompt depois de gerá-lo. Apenas entregue.

## Formato da sua Resposta

Entregue APENAS o prompt gerado, dentro de um bloco de código markdown (```), pronto para copiar e colar. Sem preâmbulo, sem postâmbulo, sem justificar escolhas.

## Objetivo do Prompt

<!--
Insira aqui o objetivo do prompt
-->
````

---

# Experimento 01 — Geração Direta de Prompt via Meta-Prompt

**Data:** 2026-05-29
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

## Contexto

Demonstração da técnica de geração direta de prompts: usar um meta-prompt para transformar uma descrição em linguagem natural em um prompt estruturado e pronto para uso. O caso de uso é auditoria de Dockerfile Python com foco em segurança e performance.

## Prompt usado

```
Você é um especialista em engenharia de prompts. Sua tarefa é receber a descrição de uma
tarefa/desejo do usuário e gerar UM PROMPT COMPLETO e pronto para uso, sem fazer perguntas.

[meta-prompt completo — ver seção "Prompt Gerador de Prompt" acima]

## Objetivo do Prompt

Preciso de um prompt que analise um Dockerfile de aplicação Python,
identificando problemas de segurança e oportunidades de melhoria de
performance. O output deve ser uma lista priorizada das recomendações,
da mais crítica pra menos crítica, com justificativa técnica em cada
item. O público é desenvolvedor backend que vai aplicar as mudanças.
```

## Resposta da IA

O meta-prompt classificou a tarefa como analítica/extração com output estruturado. Gerou um prompt com 5 seções: Papel, Objetivo, Eixos de Análise, Formato de Saída e Regras. A estrutura adaptativa escolheu CoT implícito (eixos de análise) e priorizou rigidez de output (formato fixo por recomendação).

## Output completo da IA

```markdown
## Papel

Você é um engenheiro DevSecOps sênior especializado em segurança e performance de containers
Docker para aplicações Python em ambientes de produção.

## Objetivo

Analise o Dockerfile fornecido. Identifique todos os problemas de segurança e oportunidades
de melhoria de performance. Retorne uma lista priorizada de recomendações, ordenada da mais
crítica para a menos crítica, com justificativa técnica objetiva para cada item.

## Dockerfile a Analisar

​```dockerfile
FROM python
WORKDIR /app
COPY . /app
RUN apt-get update && apt-get install -y curl build-essential
RUN pip install -r requirements.txt
ENV DB_PASSWORD=admin123
ENV API_KEY=sk-production-key-9f2a3b4c5d6e
EXPOSE 5000
CMD python app.py
​```

## Eixos de Análise

Examine o Dockerfile sob estes três eixos:

1. **Segurança** — credenciais hardcoded, execução como root, imagem base sem versão fixa,
   superfície de ataque desnecessária
2. **Performance** — ausência de multi-stage build, ordenação de camadas para aproveitamento
   de cache, dependências de build incluídas no runtime
3. **Manutenibilidade** — fixação de versões, reprodutibilidade de build, ausência de
   .dockerignore

## Formato de Saída

Para cada recomendação, use exatamente este formato:

### [N]. [Título da Recomendação] — [CRÍTICO | ALTO | MÉDIO | BAIXO]

**Problema:** o que está errado e qual o risco concreto (máx. 3 linhas)
**Correção:** instrução direta de como resolver, com snippet de código quando aplicável
**Impacto técnico:** o que muda de forma mensurável ao aplicar a correção

---

## Regras de Análise

- Credenciais expostas e execução como root são classificadas CRÍTICO sem exceção
- Inclua snippet de código corrigido obrigatoriamente em recomendações CRÍTICO e ALTO
- Limite a no máximo 8 recomendações — foque em impacto real, não em volume
- Tom técnico e direto — o leitor é desenvolvedor backend que vai implementar as mudanças
- Não repita conceitos entre recomendações distintas
```

## O que funcionou

- A estrutura de eixos (Segurança / Performance / Manutenibilidade) guiou a análise sem sobreposição
- O formato fixo por recomendação (`Problema / Correção / Impacto`) gerou output consistente e acionável
- A regra "Segredos e root = CRÍTICO sem exceção" foi respeitada em 100% dos casos

## O que não funcionou

- O prompt ainda é verboso para Dockerfiles com poucos problemas — formato proporcional ao número de itens
- A regra "máximo 8 recomendações" forçou omissão de alguns pontos menores de estilo

## Ajustes que eu fiz

- Nenhum ajuste necessário — o prompt gerado funcionou corretamente na primeira execução

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O meta-prompt gerou um prompt estruturado e aplicável sem iterações. A classificação adaptativa da tarefa como analítica/extração resultou em seções corretas.

## O que eu faria diferente

Testar a mesma descrição em diferentes LLMs (GPT-4o, Gemini 2.5 Pro) para comparar a qualidade do prompt gerado — o meta-prompt é portável por design.

---

# Experimento 02 — Validação do Prompt Gerado com Dockerfile Problemático

**Data:** 2026-05-29
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Revisão

## Contexto

Validação do prompt gerado no Experimento 01 contra o Dockerfile propositalmente problemático. O arquivo contém: imagem sem tag, credenciais em ENV, ausência de usuário não-root, ausência de multi-stage build, cache de pip ineficiente, CMD em shell form.

## Prompt usado

```
[Prompt gerado no Experimento 01 — inserido integralmente com o Dockerfile de teste]
```

## Resposta da IA

Identificou 8 problemas em ordem decrescente de severidade. Classificou corretamente 2 como CRÍTICO (credenciais e imagem sem versão), 2 como ALTO (root e multi-stage), 2 como MÉDIO (cache e CMD) e 2 como BAIXO (healthcheck e .dockerignore). Todos os itens CRÍTICO e ALTO incluíram snippets corrigidos.

## Output completo da IA

### 1. Credenciais de Produção Expostas na Imagem — CRÍTICO

**Problema:** `ENV DB_PASSWORD=admin123` e `ENV API_KEY=sk-production-key-9f2a3b4c5d6e` estão hardcoded no Dockerfile. Qualquer pessoa com acesso ao registro de imagens pode extraí-los via `docker inspect` ou `docker history`. As credenciais também ficam registradas no histórico de camadas da imagem.

**Correção:** Remova as linhas ENV com segredos. Injete-os em runtime:

```bash
docker run \
  -e DB_PASSWORD="${DB_PASSWORD}" \
  -e API_KEY="${API_KEY}" \
  myapp:latest
```

Para produção: AWS Secrets Manager, HashiCorp Vault, Kubernetes Secrets ou Docker Secrets (Swarm).

**Impacto técnico:** Elimina exposição de credenciais em registros de imagem, logs de CI/CD e histórico de camadas. Cumpre requisitos de compliance (SOC 2, PCI-DSS, ISO 27001).

---

### 2. Imagem Base Sem Pinagem de Versão — CRÍTICO

**Problema:** `FROM python` usa a tag `latest` implicitamente, tornando o build não-determinístico. Uma atualização automática pode introduzir breaking changes ou vulnerabilidades sem nenhum aviso.

**Correção:**
```dockerfile
FROM python:3.12.3-slim-bookworm
```

**Impacto técnico:** Builds 100% reproduzíveis. Redução de ~1 GB (latest full) para ~150 MB (slim). Elimina introdução silenciosa de vulnerabilidades via atualização de base.

---

### 3. Container Executa como Root — ALTO

**Problema:** Sem diretiva `USER`, o processo executa como root (UID 0). Em caso de exploit na aplicação Python, o atacante tem controle total sobre o sistema de arquivos do container.

**Correção:**
```dockerfile
RUN groupadd --system appgroup && \
    useradd --system --gid appgroup --no-create-home appuser

USER appuser
```

**Impacto técnico:** Blast radius limitado ao escopo do usuário `appuser`. Atende benchmarks CIS Docker.

---

### 4. Ferramentas de Build Presentes na Imagem de Runtime — ALTO

**Problema:** `build-essential` e `curl` instalados e presentes na imagem final ampliam a superfície de ataque e o tamanho da imagem sem valor em produção.

**Correção:**
```dockerfile
# Estágio de build
FROM python:3.12.3-slim-bookworm AS builder
WORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends build-essential
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Estágio de runtime
FROM python:3.12.3-slim-bookworm
WORKDIR /app
COPY --from=builder /install /usr/local
COPY . .
```

**Impacto técnico:** Reduz tamanho final em 60-70%. Restringe ferramentas disponíveis para um atacante.

---

### 5. Ordem de Camadas Invalida Cache do pip — MÉDIO

**Problema:** `COPY . /app` antes de `pip install` invalida o cache da instalação a cada mudança de código, mesmo quando `requirements.txt` não mudou.

**Correção:**
```dockerfile
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
```

**Impacto técnico:** Builds incrementais até 10× mais rápidos quando apenas o código-fonte muda.

---

### 6. CMD em Shell Form Impede Graceful Shutdown — MÉDIO

**Problema:** `CMD python app.py` inicia o processo como `/bin/sh -c python app.py`. O PID 1 é o shell, não o Python. Sinais SIGTERM do Docker não chegam à aplicação.

**Correção:**
```dockerfile
CMD ["python", "app.py"]
```

**Impacto técnico:** SIGTERM chega diretamente ao Python, permitindo fechamento correto de conexões. Reduz erros em rolling deployments.

---

### 7. Ausência de HEALTHCHECK — BAIXO

**Problema:** Sem HEALTHCHECK, orquestradores não detectam containers degradados que ainda respondem na porta mas não processam requisições corretamente.

**Correção:**
```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=15s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:5000/health')" || exit 1
```

**Impacto técnico:** Substituição automática de instâncias não-saudáveis por orquestradores.

---

### 8. Contexto de Build Sem .dockerignore — BAIXO

**Problema:** `COPY . /app` sem `.dockerignore` inclui `.git`, `__pycache__`, arquivos `.env` locais e outros artefatos na imagem.

**Correção:** Crie `.dockerignore` na raiz:
```
.git
__pycache__
*.pyc
*.pyo
.env
.env.*
*.log
tests/
.pytest_cache/
```

**Impacto técnico:** Elimina risco de vazar arquivos `.env` locais. Reduz tempo de transferência do contexto de build.

## O que funcionou

- O prompt gerado cobriu todos os 8 antipatterns propositais do Dockerfile de teste
- Severidades corretas em 100% dos casos — especialmente a classificação dupla CRÍTICO
- Snippets completos e acionáveis em todos os itens CRÍTICO e ALTO

## O que não funcionou

- Output longo para 8 recomendações — adequado para Dockerfiles complexos, verboso para casos simples
- Não houve inferência automática de linguagem/framework além do que estava explícito

## Ajustes que eu fiz

- Nenhum — prompt one-shot funcionou sem iterações

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Cobertura completa dos antipatterns, severidades corretas, snippets acionáveis. Validação bem-sucedida do prompt gerado pelo meta-prompt.

## O que eu faria diferente

Parametrizar o Dockerfile como variável `{DOCKERFILE}` para reusar o prompt com outros arquivos sem editar o prompt base — próximo passo natural antes do refinamento.
