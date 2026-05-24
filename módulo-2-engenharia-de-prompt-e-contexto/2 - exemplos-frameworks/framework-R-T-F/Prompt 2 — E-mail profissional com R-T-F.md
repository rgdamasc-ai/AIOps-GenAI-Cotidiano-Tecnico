# Experimento 02 — E-mail profissional com R-T-F

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Mesmo cenário do Experimento 01 (solicitação de aumento de orçamento), agora reestruturado com o framework R-T-F. Role define o perfil do remetente, Task especifica o destinatário, o percentual e a justificativa, Format determina a estrutura e o limite de palavras.

## Prompt usado

```markdown
# Role
Você é um gerente de projetos experiente que sabe comunicar decisões de forma objetiva e persuasiva.

# Task
Escreva um e-mail para o diretor de TI solicitando aumento de 30% no orçamento do projeto de migração cloud, justificando com base em escopo ampliado e riscos de atraso.

# Format
E-mail profissional com: linha de assunto, saudação, 3 parágrafos (contexto, justificativa com dados, próximos passos) e encerramento. Máximo 200 palavras.
```

## Resposta da IA

A IA gerou um e-mail completo, estruturado e persuasivo dentro do limite de palavras solicitado. O assunto é específico, os três parágrafos cumprem exatamente as funções definidas no Format e o tom é profissional e assertivo, alinhado com o Role de gerente de projetos.

## Output completo da IA

```
Assunto: Solicitação de Revisão Orçamentária — Projeto de Migração Cloud (+30%)

Prezado [Nome do Diretor de TI],

O projeto de migração cloud encontra-se em fase crítica de execução. Desde o início, o escopo foi ampliado para incluir a migração de três sistemas legados adicionais, identificados durante o levantamento técnico, o que impacta diretamente os recursos necessários para entrega no prazo acordado.

Diante disso, solicito formalmente um reajuste de 30% no orçamento vigente. Sem esse reforço, o projeto corre risco de atraso estimado em 6 semanas, com impacto direto no cronograma de go-live e nos contratos de SLA já firmados com as áreas de negócio.

Sugiro uma reunião nos próximos 5 dias úteis para apresentar o detalhamento dos custos e validar os ajustes necessários. Coloco-me à disposição para alinhar a melhor data.

Atenciosamente,
[Nome]
Gerente de Projetos
```

## O que funcionou

- A estrutura de 3 parágrafos foi respeitada com precisão (contexto → justificativa → próximos passos)
- O assunto já comunica o tema e o percentual solicitado
- O tom é persuasivo sem ser agressivo, alinhado ao perfil do Role
- Limite de 200 palavras respeitado sem perda de conteúdo essencial
- Nenhuma reformulação necessária — o resultado foi utilizável na primeira tentativa

## O que não funcionou

- Os valores reais (orçamento atual e valor com reajuste) precisariam ser preenchidos manualmente — a IA sinalizou isso com `[R$ X]`
- O nome do diretor e do gerente estão como placeholders, o que é esperado dado que não foram fornecidos no prompt

## Ajustes que eu fiz

- Nenhum ajuste foi necessário no prompt — o R-T-F entregou o resultado esperado na primeira execução
- Em uso real, bastaria adicionar os nomes e valores ao prompt para eliminar os placeholders

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O e-mail é profissional, persuasivo, dentro do limite e pronto para uso com ajustes mínimos. O contraste com o Prompt 1 demonstra claramente o impacto do framework R-T-F: de um rascunho inutilizável para um documento utilizável na primeira tentativa.

## O que eu faria diferente

Para uso real, incluiria os valores concretos diretamente no Task (ex: "orçamento atual de R$ 150.000, solicitando R$ 195.000") e o nome do destinatário, eliminando a necessidade de edição posterior.
