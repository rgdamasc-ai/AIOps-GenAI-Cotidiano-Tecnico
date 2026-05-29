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

# Experimento 03 — Refinamento de Prompt com Meta-Prompt Refinador

**Data:** 2026-05-29
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

## Contexto

Aplicação do meta-prompt refinador ao prompt de auditoria de Dockerfile gerado na Aula 1. Objetivo: reduzir consumo de tokens (de ~2k input / ~8k output) e adicionar retorno de Dockerfile corrigido como segundo entregável.

## Prompt usado

```
Você deve atuar como um engenheiro de prompt responsável por aprimorar
prompts já construídos.

Receba o prompt, faça uma análise do conteúdo desse prompt e analise
o que o usuário quer que seja alterado.

Depois dessa reflexão, eu quero que você retorne um prompt atendendo
às expectativas do usuário em um formato de arquivo Markdown.

Prompt:
## Papel
Você é um engenheiro DevSecOps sênior especializado em segurança e performance de containers
Docker para aplicações Python em ambientes de produção.

## Objetivo
Analise o Dockerfile fornecido. Identifique todos os problemas de segurança e oportunidades
de melhoria de performance. Retorne uma lista priorizada de recomendações, ordenada da mais
crítica para a menos crítica, com justificativa técnica objetiva para cada item.

## Dockerfile a Analisar
[Dockerfile completo — ver geração-direta.md]

## Eixos de Análise
1. Segurança — credenciais hardcoded, execução como root, imagem base sem versão fixa, superfície de ataque desnecessária
2. Performance — ausência de multi-stage build, ordenação de camadas para aproveitamento de cache, dependências de build incluídas no runtime
3. Manutenibilidade — fixação de versões, reprodutibilidade de build, ausência de .dockerignore

## Formato de Saída
[formato por recomendação com Problema / Correção / Impacto técnico]

## Regras de Análise
- Credenciais expostas e execução como root são classificadas CRÍTICO sem exceção
- Inclua snippet de código corrigido obrigatoriamente em recomendações CRÍTICO e ALTO
- Limite a no máximo 8 recomendações
- Tom técnico e direto
- Não repita conceitos entre recomendações distintas

Melhorias:
- Eu quero que o consumo de tokens seja reduzido. Hoje consome
  aproximadamente 2 mil tokens de input e 8 mil tokens de output.
- Além da análise, deve retornar um Dockerfile seguindo as boas práticas.

Output:
Retorne apenas o prompt alterado em um arquivo Markdown.
```

## Resposta da IA

O refinador comprimiu o prompt de 6 seções para 4, fundindo "Eixos de Análise" com "Regras" e tornando o formato de saída mais enxuto. Adicionou a "Parte 2 — Dockerfile Corrigido" como entregável obrigatório. Reduziu o vocabulário de instrução em ~40% mantendo a cobertura funcional.

## Output completo da IA

```markdown
## Papel

Auditor DevSecOps — containers Docker para Python.

## Objetivo

Analise o Dockerfile abaixo. Retorne:
1. Lista priorizada de problemas (CRÍTICO → BAIXO) com justificativa técnica
2. Dockerfile corrigido seguindo as boas práticas identificadas

## Dockerfile

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

## Formato de Saída

**Parte 1 — Recomendações**

### [N]. [Título] — [CRÍTICO | ALTO | MÉDIO | BAIXO]

- **Problema:** risco ou ineficiência em 1-2 linhas
- **Correção:** instrução direta + snippet (obrigatório para CRÍTICO e ALTO)

**Parte 2 — Dockerfile Corrigido**

Dockerfile completo, multi-stage quando aplicável, com comentário de 1 linha por decisão relevante.

## Regras

- Segredos hardcoded e execução como root = CRÍTICO sem exceção
- Máximo 6 recomendações, por ordem de impacto real em produção
- Snippets em todas as recomendações CRÍTICO e ALTO
- Tom técnico direto; público: dev backend que vai aplicar as mudanças
```

## O que funcionou

- Redução de ~40% no volume de instrução sem perda de cobertura funcional
- Fusão de seções ("Eixos de Análise" + "Regras") eliminou redundância sem perder semântica
- A adição de "Parte 2 — Dockerfile Corrigido" como entregável explícito ficou clara e acionável
- Limite de 6 recomendações (vs. 8 original) força priorização mais rigorosa

## O que não funcionou

- A seção "Eixos de Análise" foi eliminada na fusão — em Dockerfiles mais complexos, a guia de análise explícita pode fazer falta
- O prompt refinado não preservou a instrução de "não repetir conceitos entre recomendações"

## Ajustes que eu fiz

- Nenhum ajuste aplicado — o refinamento atendeu ambas as melhorias solicitadas

## Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** Alcançou os dois objetivos de refinamento (tokens e Dockerfile corrigido). Desconto de 1 ponto pela perda da guia explícita de eixos de análise, que contribuía para cobertura sistemática.

## O que eu faria diferente

Solicitar ao refinador que mantenha os eixos de análise como checklist interno (não exibido no output) em vez de removê-los completamente — reduz tokens sem perder profundidade analítica.
