# Experimento 01 — E-mail profissional sem estrutura

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Teste de prompt sem framework R-T-F para solicitar um e-mail profissional. O objetivo é demonstrar o resultado genérico obtido quando não há role, task ou format definidos.

## Prompt usado

```
Me escreve um e-mail pedindo aumento de orçamento
```

## Resposta da IA

A IA gerou um e-mail curto, vago e sem argumentos. Sem definição de destinatário, sem valor solicitado, sem justificativa e sem estrutura profissional. O tom foi neutro e impessoal, mas sem persuasão.

## Output completo da IA

```
Assunto: Solicitação de Aumento de Orçamento

Prezado(a),

Gostaria de solicitar formalmente um aumento no orçamento do projeto. Conforme o andamento atual, percebemos que os recursos disponíveis não são suficientes para atender às demandas previstas para a próxima fase.

Agradeço a atenção e aguardo retorno.

Atenciosamente,
[Nome]
```

## O que funcionou

- A IA entendeu o pedido básico e gerou um e-mail funcional
- A estrutura mínima (saudação + corpo + encerramento) foi respeitada

## O que não funcionou

- Nenhum argumento ou dado para justificar o pedido
- Sem destinatário definido, sem valor ou percentual solicitado
- Tom genérico, sem persuasão nem contexto de negócio
- Assunto pouco específico, não diferencia o projeto ou urgência
- Precisaria de ao menos 3 reformulações para chegar em algo utilizável

## Ajustes que eu fiz

- Nenhum ajuste foi feito — o prompt foi enviado exatamente como estava para demonstrar o problema da ausência do framework R-T-F

## Nota (1 a 5)

**Nota:** 2/5

**Justificativa:** A IA entregou um rascunho mínimo, mas inutilizável em contexto profissional real. Faltam todos os elementos de um e-mail persuasivo: quem pede, para quem, quanto, por quê e qual o próximo passo.

## O que eu faria diferente

Aplicar o framework R-T-F desde o início: definir o papel de quem escreve (gerente de projetos), a tarefa com dados concretos (percentual, destinatário, justificativa) e o formato esperado (número de parágrafos, limite de palavras). Isso é exatamente o que o Prompt 2 demonstra.
