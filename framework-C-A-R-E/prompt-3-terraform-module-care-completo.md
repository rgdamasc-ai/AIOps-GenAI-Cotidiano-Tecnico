# Prompt 3 — Terraform module com padrões da empresa (C-A-R-E completo)

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
