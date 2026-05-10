# CRIA ESTRUTURA DE PASTAS E README

# Role
Você é um especialista gitops

# Contexto
Repositório no Github com o nome "AIOps-GenAI-Cotidiano-Tecnico" e vs code

# Instrução
Crie a pasta "framework-C-A-R-E" e dentro dela arquivos com os nomes abaixo, deixando-os padronizados .md diretamente no vs code.

Nomes para usar:

Prompt 1 — Feedback de desempenho SEM Example
Prompt 2 — Feedback de desempenho COM Example (C-A-R-E completo)
Prompt 3 — Terraform module com padrões da empresa (C-A-R-E completo)

Após criar os arquivos, crie um documento Readme.md dentro da pasta "framework-C-A-R-E" com todo o conteúdo abaixo em formato Markdown:


## Prompt 1 — Feedback de desempenho SEM Example

**Cenário:** Feedback semestral para uma engenheira plena com Context, Action e Result, mas sem Example. Resultado vem em formato genérico, sem tom consistente com a empresa.

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo.
```

## Prompt 2 — Feedback de desempenho COM Example (C-A-R-E completo)

**Cenário:** Mesmo cenário com Example fornecido (feedback do ciclo passado). O modelo replica o tom direto, exemplos concretos e formato de metas mensuráveis.

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo. Tom construtivo e direto, sem jargão corporativo.

# Example
Aqui está um feedback que escrevi no ciclo passado para outro membro do time, no formato e tom que quero manter:

**Pontos Fortes:**
O João foi peça-chave na entrega do projeto de migração do monólito. Ele liderou a definição da arquitetura de microsserviços e conduziu 3 design reviews com o time. Destaque para a documentação dos ADRs — ficou clara e serviu de referência para decisões futuras.

**Áreas de Desenvolvimento:**
O João precisa desenvolver mais autonomia na priorização. Em duas situações no trimestre, ele esperou direcionamento meu para decidir entre tarefas que poderia ter priorizado sozinho. Sugestão: usar a matriz de impacto/esforço antes de escalar.

**Próximos Passos:**
- Liderar pelo menos 1 design review por sprint sem acompanhamento do tech lead (até março)
- Priorizar autonomamente tarefas de até 3 story points sem escalar (próximo ciclo)
```

## Prompt 3 — Terraform module com padrões da empresa (C-A-R-E completo)

**Cenário:** Geração de Terraform module para bucket S3 com versionamento, encryption KMS e lifecycle. Example traz module de SQS já em produção como referência de padrões (tags, validações, naming kebab-case).

````markdown
# Context
Trabalhamos com Terraform na AWS. Seguimos o padrão de módulos do terraform-aws-modules como referência de estrutura. Usamos workspaces para separar ambientes (dev, staging, prod). Backend em S3 com DynamoDB para state lock. Convenção de naming: kebab-case para recursos, snake_case para variáveis.

# Action
Crie um Terraform module para provisionar um bucket S3 com versionamento, encryption com KMS e lifecycle rules para transição para Glacier após 90 dias.

# Result
O module deve conter: main.tf, variables.tf com descrições e validações, outputs.tf com ARN e nome do bucket, e um README.md com exemplo de uso. Seguir as convenções de naming definidas no contexto.

# Example
Aqui está a estrutura do nosso module de SQS que já está em produção, como referência de padrão:

```hcl
# modules/sqs-queue/main.tf
resource "aws_sqs_queue" "this" {
  name                       = var.queue_name
  delay_seconds              = var.delay_seconds
  max_message_size           = var.max_message_size
  message_retention_seconds  = var.message_retention_seconds
  visibility_timeout_seconds = var.visibility_timeout_seconds

  tags = merge(var.tags, {
    Module = "sqs-queue"
  })
}

# modules/sqs-queue/variables.tf
variable "queue_name" {
  description = "Nome da fila SQS"
  type        = string

  validation {
    condition     = can(regex("^[a-z][a-z0-9-]+$", var.queue_name))
    error_message = "O nome da fila deve usar kebab-case."
  }
}
```
````
Antes de criar, mostre na tela e peça aprovação


=======================================

# EXECUTA PERGUNTAS OBTEM RESPOSTAS E GERA MARKDOWN

# Role
Você é um especialista gitops com amplo conhecimento em IA Claude Code

# Contexto
Github com o repositório "AIOps-GenAI-Cotidiano-Tecnico", pasta "framework-C-A-R-E" e dentro todos os aquivos abaixo inclusive com o README.md:

Nomes para usar:

Prompt 1 — Feedback de desempenho SEM Example
Prompt 2 — Feedback de desempenho COM Example (C-A-R-E completo)
Prompt 3 — Terraform module com padrões da empresa (C-A-R-E completo)

# Instrução
Execute os prompts de cada cenário do arquivo readme.md dentro da pasta "framework-C-A-R-E", um de cada vez, aguarde retornar o output de cada prompt e com o resultado, utilize o arquivo "template-do-caderno-de-experimentos.md" para registrar o markdown usando-o como modelo padrão. Inclua o arquivo markdown dentro de cada arquivo relaciondo no # Contexto

# Output
retorne na tela cada arquivo criado em markdown solicitando aprovação

=============================================

# CRIA MODELO PARA ENGENHARIA DE PROMPT

# Role
Você é um especialista gitops

# Contexto
Repositório no Github com o nome "AIOps-GenAI-Cotidiano-Tecnico", usando vs code.

# Instrução
Crie o arquivo "engenharia-de-prompt-C-A-R-E.md" com o exemplo de prompt abaixo e insira na raiz do projeto "AIOps-GenAI-Cotidiano-Tecnico":

Quando utilizar o Framework C-A-R-E (Context-After-Result-Example)

O framework C-A-R-E deve ser utilizado quando você possui um exemplo de referência claro para guiar a IA, como em feedbacks estruturados, módulos de infraestrutura (Terraform) ou revisões de código baseadas em padrões internos. Evite-o em tarefas de resposta curta, brainstorms criativos ou quando não houver um modelo, pois o esforço do prompt não justifica o resultado nessas situações.

```
# Context
[Espaço para texto]

# Action
[Espaço para texto]

# Result
[Espaço para texto]

# Example
[Espaço para texto]

```
# output
Finalize com o formato markdown
