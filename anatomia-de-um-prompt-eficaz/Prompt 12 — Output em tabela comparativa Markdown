# Experimento 12 — Output em tabela comparativa Markdown

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma pergunta do Prompt 11, com instrução explícita de formato: tabela Markdown com 4 colunas definidas, limite de 5 serviços e ordenação por complexidade crescente. Demonstra como a especificação de Output transforma a resposta em algo diretamente utilizável em documentação.

## Prompt usado

```
Liste os serviços AWS para hospedar containers.

Formato: tabela Markdown com colunas Serviço | Tipo (serverless/managed) | Caso de uso ideal | Custo relativo.
Máximo 5 serviços. Ordene do mais simples ao mais complexo.
```

## Resposta da IA

O modelo retornou uma tabela Markdown com 5 linhas ordenadas por complexidade crescente: App Runner → ECS Fargate → ECS com EC2 → EKS Fargate → EKS com EC2. Cada linha preencheu exatamente as 4 colunas solicitadas (Serviço, Tipo, Caso de uso ideal, Custo relativo). Sem texto adicional fora da tabela.

## Output completo da IA

| Serviço | Tipo | Caso de uso ideal | Custo relativo |
|---|---|---|---|
| App Runner | Serverless | APIs e apps web simples, sem configuração de infra | Baixo |
| ECS Fargate | Serverless | Microsserviços sem gerenciar servidores, integração nativa AWS | Médio |
| ECS com EC2 | Managed | Workloads com necessidade de controle de instâncias e otimização de custo | Médio-baixo |
| EKS Fargate | Serverless | Kubernetes sem nodes, workloads Kubernetes simples | Médio-alto |
| EKS com EC2 | Managed | Kubernetes completo, grandes volumes, portabilidade multi-cloud | Alto |

## O que funcionou

- Tabela pronta para colar em README, Confluence ou apresentação — zero reprocessamento
- Ordenação por complexidade crescente respeitada, criando uma narrativa de progressão natural
- O modelo inferiu corretamente que ECS com EC2 é "Managed" (não serverless) e o posicionou adequadamente
- Limite de 5 serviços forçou uma curadoria: o modelo priorizou os mais relevantes para o caso geral

## O que não funcionou

- "Custo relativo" é subjetivo — valores como "Médio-alto" dependem do volume de uso. Uma nota de rodapé com premissas tornaria isso mais preciso
- ECS com EC2 aparece antes do EKS Fargate na ordem de complexidade, o que pode variar dependendo da perspectiva (operacional vs. conceitual)

## Ajustes que eu fiz

- Apenas adicionadas as instruções de formato, limite e ordenação em relação ao Prompt 11
- O corpo do pedido é idêntico

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** A instrução de Output transformou texto corrido em tabela pronta para uso. O modelo respeitou todas as restrições (4 colunas, 5 linhas, ordenação) sem nenhuma reformulação necessária.

## O que eu faria diferente

Adicionar uma linha de instrução pedindo uma nota de rodapé com as premissas do "Custo relativo" (ex: "assumindo workloads de produção com tráfego moderado") para tornar a comparação auditável.
