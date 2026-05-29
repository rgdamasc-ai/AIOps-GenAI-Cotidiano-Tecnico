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
# Aula 2 - Refinamento de Prompt

Esta aula demonstra como usar um meta-prompt para refinar um prompt já existente — em vez de gerar do zero. O instrutor parte do prompt de auditoria de Dockerfile construído nas aulas anteriores e pede à IA para refiná-lo com objetivos específicos: reduzir consumo de tokens e ajustar o output esperado.

---

## Meta-Prompt — Engenheiro de Prompt (refinador)

Meta-prompt em formato persona que recebe um prompt existente e instruções de melhoria, e devolve a versão refinada em Markdown. Cole o prompt original no campo `[prompt original a ser refinado]` e ajuste a seção `Melhorias` conforme o que precisa mudar (custo, formato, escopo, persona, etc.).

```
Você deve atuar como um engenheiro de prompt responsável por aprimorar
prompts já construídos.

Receba o prompt, faça uma análise do conteúdo desse prompt e analise
o que o usuário quer que seja alterado.

Depois dessa reflexão, eu quero que você retorne um prompt atendendo
às expectativas do usuário em um formato de arquivo Markdown.

Prompt:
[prompt original a ser refinado]

Melhorias:
- Eu quero que o consumo de tokens seja reduzido. Hoje consome
  aproximadamente 2 mil tokens de input e 8 mil tokens de output.
- Além da análise, deve retornar um Dockerfile seguindo as boas práticas.

Output:
Retorne apenas o prompt alterado em um arquivo Markdown.
```

---
# Aula 3 - Transformação de Prompt

Esta aula demonstra um meta-prompt que recebe um prompt-base e o reescreve aplicando uma ou mais técnicas (Chain-of-Thought, Tree-of-Thoughts, etc.), gerando versões comparáveis em arquivos Markdown separados. A vantagem é poder comparar técnicas executando a mesma tarefa com prompts equivalentes.

---

## Meta-Prompt de Transformação por Técnica

Meta-prompt em formato persona. Recebe o prompt-base (no caso da aula, o Dockerfile Auditor refinado na Aula 04) e uma lista de técnicas. Devolve um arquivo Markdown para cada técnica solicitada, mantendo o contexto e objetivo original mas estruturando o raciocínio de acordo com a técnica.

```
Você é um especialista em engenharia de prompt e o seu papel é receber
determinado prompt inserido pelo usuário e fazer a transformação desse
prompt utilizando técnicas que o usuário vai solicitar.

Você deve sempre manter o contexto e o objetivo do prompt, mas
utilizando ele como base para criar as técnicas definidas para poder
gerar uma base de comparação e analisar qual vai ter uma melhor
performance.

[prompt-base a ser transformado]

Técnicas: Chain of Thoughts, Tree of Thoughts
Output: criar um arquivo markdown para cada técnica solicitada
```
