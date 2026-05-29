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
