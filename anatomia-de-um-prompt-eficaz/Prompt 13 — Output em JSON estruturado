# Experimento 13 — Output em JSON estruturado

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma pergunta dos Prompts 11 e 12, agora com formato JSON e filtro de conteúdo (apenas serviços serverless). Demonstra como o formato de output pode simultaneamente estruturar a resposta para integração com sistemas e filtrar o conteúdo relevante para o caso de uso.

## Prompt usado

```
Liste os serviços AWS para hospedar containers.

Formato: array JSON com objetos contendo campos "service", "type", "use_case".
Apenas serviços serverless.
```

## Resposta da IA

O modelo retornou um array JSON válido com 4 objetos (App Runner, ECS Fargate, EKS Fargate, Lambda Container Images), todos com os campos "service", "type" e "use_case" preenchidos. Filtrou corretamente ECS com EC2 e EKS com EC2, que não são serverless. Sem texto adicional fora do JSON.

## Output completo da IA

```json
[
  {
    "service": "AWS App Runner",
    "type": "serverless",
    "use_case": "APIs e aplicações web com deploy simplificado, sem configuração de infraestrutura"
  },
  {
    "service": "Amazon ECS Fargate",
    "type": "serverless",
    "use_case": "Microsserviços e workloads de longa duração sem gerenciamento de servidores"
  },
  {
    "service": "Amazon EKS Fargate",
    "type": "serverless",
    "use_case": "Workloads Kubernetes sem necessidade de gerenciar worker nodes"
  },
  {
    "service": "AWS Lambda (Container Images)",
    "type": "serverless",
    "use_case": "Funções event-driven e workloads intermitentes empacotados como imagens Docker"
  }
]
```

## O que funcionou

- JSON válido e parseável diretamente — pode ser consumido por scripts, dashboards ou pipelines de CI/CD sem reprocessamento
- Filtro "apenas serverless" respeitado: os 2 serviços managed (ECS EC2 e EKS EC2) foram excluídos corretamente
- Nomes de campos em snake_case consistentes com convenções de APIs REST
- O modelo não adicionou campos extras além dos solicitados

## O que não funcionou

- Sem schema formal, campos como "type" poderiam ter valores variados em execuções diferentes ("serverless", "Serverless", "SERVERLESS")
- "use_case" como string livre pode ter comprimentos inconsistentes — um campo de max_chars ajudaria

## Ajustes que eu fiz

- Apenas alterado o formato (tabela → JSON) e adicionado o filtro "apenas serverless" em relação ao Prompt 12
- O corpo do pedido é idêntico aos anteriores

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** JSON válido, filtrado corretamente e sem texto adicional — saída pronta para integração com outro sistema. A progressão Prompt 11 → 12 → 13 demonstra como o mesmo conteúdo pode ser moldado para diferentes destinos (leitura humana → documentação → integração de sistemas).

## O que eu faria diferente

Formalizar o schema com tipos e enums, como demonstrado no Prompt 14, para garantir consistência em chamadas repetidas ou em pipelines de automação.
