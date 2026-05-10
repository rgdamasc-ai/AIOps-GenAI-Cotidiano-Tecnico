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

---

# Experimento 03 — Terraform module com padrões da empresa (C-A-R-E completo)

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

## Contexto

Geração de Terraform module para bucket S3 com versionamento, encryption KMS e lifecycle rules.
Example traz module de SQS já em produção como referência de padrões (tags, validações, naming).

## Prompt usado

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
resource "aws_sqs_queue" "this" {
  name = var.queue_name
  tags = merge(var.tags, { Module = "sqs-queue" })
}

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

## Resposta da IA

A IA gerou um module completo com os 4 arquivos solicitados. Seguiu o padrão do Example: resource nomeado `this`, `merge(var.tags, { Module = "..." })`, validação kebab-case em variáveis, separação dos recursos em blocos distintos. O README incluiu tabela de inputs/outputs e exemplo de uso com `terraform.workspace`.

## Output completo da IA

**modules/s3-bucket/main.tf:**
```hcl
resource "aws_s3_bucket" "this" {
  bucket = var.bucket_name

  tags = merge(var.tags, {
    Module = "s3-bucket"
  })
}

resource "aws_s3_bucket_versioning" "this" {
  bucket = aws_s3_bucket.this.id
  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "this" {
  bucket = aws_s3_bucket.this.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms"
      kms_master_key_id = var.kms_key_id
    }
    bucket_key_enabled = true
  }
}

resource "aws_s3_bucket_lifecycle_configuration" "this" {
  bucket = aws_s3_bucket.this.id
  rule {
    id     = "transition-to-glacier"
    status = "Enabled"
    transition {
      days          = 90
      storage_class = "GLACIER"
    }
  }
}
```

**modules/s3-bucket/variables.tf:**
```hcl
variable "bucket_name" {
  description = "Nome do bucket S3 (kebab-case)"
  type        = string
  validation {
    condition     = can(regex("^[a-z][a-z0-9-]+$", var.bucket_name))
    error_message = "O nome do bucket deve usar kebab-case."
  }
}

variable "kms_key_id" {
  description = "ARN ou ID da chave KMS para criptografia do bucket"
  type        = string
}

variable "tags" {
  description = "Tags adicionais para os recursos"
  type        = map(string)
  default     = {}
}
```

**modules/s3-bucket/outputs.tf:**
```hcl
output "bucket_arn" {
  description = "ARN do bucket S3"
  value       = aws_s3_bucket.this.arn
}

output "bucket_name" {
  description = "Nome do bucket S3"
  value       = aws_s3_bucket.this.id
}
```

**modules/s3-bucket/README.md:**

```markdown
# Module: s3-bucket

Provisiona um bucket S3 com versionamento, encryption KMS e lifecycle para Glacier.

## Uso

module "s3-bucket" {
  source = "./modules/s3-bucket"

  bucket_name = "minha-app-artifacts-prod"
  kms_key_id  = "arn:aws:kms:us-east-1:123456789012:key/abc123"

  tags = {
    Environment = terraform.workspace
    Team        = "platform"
  }
}

## Inputs

| Nome        | Descrição                   | Tipo         | Padrão | Obrigatório |
|-------------|-----------------------------|--------------|--------|-------------|
| bucket_name | Nome do bucket (kebab-case) | string       | —      | sim         |
| kms_key_id  | ARN/ID da chave KMS         | string       | —      | sim         |
| tags        | Tags adicionais             | map(string)  | {}     | não         |

## Outputs

| Nome        | Descrição          |
|-------------|--------------------|
| bucket_arn  | ARN do bucket S3   |
| bucket_name | Nome do bucket S3  |
```

## O que funcionou

- Naming `this` para resource replicado diretamente do Example
- `merge(var.tags, { Module = "..." })` seguido sem instrução explícita
- Validação kebab-case aplicada em `bucket_name` seguindo o padrão da `queue_name`
- 4 arquivos entregues sem necessidade de iteração
- `terraform.workspace` no README demonstra absorção do Context sobre workspaces

## O que não funcionou

- Nada material — output pronto para revisão de time antes de usar em produção

## Ajustes que eu fiz

- Nenhum — output de primeira rodada

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O Example do module de SQS transmitiu os padrões da empresa de forma precisa e completa. A IA não apenas replicou a estrutura — ela generalizou o padrão corretamente para o novo recurso. Sem o Example, provavelmente teria gerado um module funcional mas fora dos padrões internos (naming diferente, sem validações, sem o merge de tags).

## O que eu faria diferente

Nada a mudar. O aprendizado: para geração de código com padrões de empresa, um Example de código em produção é mais eficaz do que qualquer descrição textual de convenções. "Aqui está como a gente faz" supera "faça seguindo kebab-case e tags merge".
