# Least to Most

Os prompts abaixo demonstram duas variações da técnica: decomposição por subproblemas com dependências explícitas e decomposição por milestones. Execute cada prompt de resolução na mesma sessão de chat, em continuidade ao prompt de decomposição anterior.

---

## Decomposição de infraestrutura AWS em subproblemas

Primeiro prompt da Demo 1. O modelo retorna apenas a lista de subproblemas agrupados por dependência, sem implementar nada. O "Retorne apenas a lista" é intencional: evita que o modelo tente resolver tudo de uma vez.

```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Decomponha a criação dessa infraestrutura em subproblemas agrupados
por dependência. Para cada subproblema, liste o que ele envolve e
de quais subproblemas anteriores ele depende. Ordene do mais
fundamental ao mais complexo.
Retorne apenas a lista.
```

---

## Resolução do Subproblema 1 (Networking/VPC)

Enviado na sequência da decomposição, na mesma janela de contexto. O contexto acumulado substitui a necessidade de repetir toda a descrição da infra.

```
Estou criando a infraestrutura na AWS para uma aplicação web: API em
containers (EKS), PostgreSQL (RDS), Redis (ElastiCache), API Gateway.
Multi-AZ, com ambientes staging e produção separados.

Resolva o subproblema 1.
```

---

## Resolução do próximo subproblema pendente

Variação minimalista para continuar a cadeia após resolver o Subproblema 1. O modelo usa o contexto da conversa para identificar qual é o próximo subproblema ainda não resolvido.

```
Resolva o próximo subproblema pendente.
```

---

## Decomposição de infraestrutura AWS em milestones

Variação da Demo 1 com formato de milestones e sub-tarefas ordenados por dependência. Gera uma estrutura mais próxima de um plano de projeto do que de uma lista técnica de recursos.

```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Crie uma lista de milestones com tarefas e sub-tarefas para a criação
dessa infraestrutura. Organize por ordem de execução, considerando
dependências entre os milestones. Retorne apenas a lista.
```

---

## Execução progressiva de milestone

Prompt de execução da Demo de milestones. Enviado na sequência da decomposição anterior para que o modelo expanda e implemente um milestone por vez.

```
Execute a próxima tarefa.
```

---

## Caderno de Experimentos

---

### Experimento 03 — Least-to-Most: Decomposição por Subproblemas

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

#### Contexto

Demonstração da técnica Least-to-Most aplicada ao planejamento de infraestrutura AWS. O modelo primeiro decompõe o problema em subproblemas com dependências explícitas, depois resolve um por vez na mesma janela de contexto. A instrução "Retorne apenas a lista" é o mecanismo central que impede o modelo de tentar resolver tudo de uma vez.

#### Prompt usado

**Prompt de Decomposição:**
```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Decomponha a criação dessa infraestrutura em subproblemas agrupados
por dependência. Para cada subproblema, liste o que ele envolve e
de quais subproblemas anteriores ele depende. Ordene do mais
fundamental ao mais complexo.
Retorne apenas a lista.
```

**Resolução — Subproblema 1:**
```
Estou criando a infraestrutura na AWS para uma aplicação web: API em
containers (EKS), PostgreSQL (RDS), Redis (ElastiCache), API Gateway.
Multi-AZ, com ambientes staging e produção separados.

Resolva o subproblema 1.
```

**Continuação:**
```
Resolva o próximo subproblema pendente.
```

#### Resposta da IA

**Decomposição:** Lista de 10 subproblemas ordenados por dependência. O modelo respeitou a instrução "Retorne apenas a lista" — não implementou nada, apenas estruturou.

**Subproblema 1 (VPC):** Terraform completo para VPC multi-AZ com subnets públicas/privadas, NAT Gateways e route tables parametrizados por `var.environment`.

**Subproblema 2 (IAM):** Roles e policies para EKS cluster, nodes e ECR, usando o contexto acumulado da janela para não repetir a descrição completa da infra.

#### Output completo da IA

**Output da Decomposição:**

1. **Networking/VPC** — VPC multi-AZ, subnets públicas e privadas (2 AZs), internet gateway, NAT gateways (1 por AZ), route tables. *Sem dependências.*

2. **IAM e Permissões** — Roles para EKS (cluster role, node role), RDS, ElastiCache. Políticas de acesso mínimo. *Depende de: 1*

3. **Security Groups** — SGs para ALB, EKS nodes, RDS (acesso apenas de EKS nodes), ElastiCache. *Depende de: 1*

4. **RDS PostgreSQL** — Instâncias multi-AZ, parameter groups, subnet groups (subnets privadas), backups. Staging e produção separados. *Depende de: 1, 2, 3*

5. **ElastiCache Redis** — Cluster mode, multi-AZ, subnet groups. Staging e produção separados. *Depende de: 1, 2, 3*

6. **EKS Cluster** — Control plane, managed node groups (multi-AZ), add-ons (CoreDNS, kube-proxy, VPC CNI), OIDC provider. *Depende de: 1, 2, 3*

7. **Container Registry (ECR)** — Repositórios, lifecycle policies, permissões de pull para nodes EKS. *Depende de: 2, 6*

8. **API Gateway** — HTTP API, stages (staging/prod), VPC Link para ALB/EKS, throttling, logging. *Depende de: 1, 3, 6*

9. **Observabilidade** — CloudWatch dashboards para EKS, RDS, ElastiCache; alarmes críticos; log aggregation. *Depende de: 4, 5, 6, 8*

10. **CI/CD e Deploy** — Pipelines por ambiente, rollout strategy, Terraform state separado. *Depende de: 1–9*

---

**Output da Resolução — Subproblema 1 (VPC):**

```hcl
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  tags = { Name = "${var.environment}-vpc" }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  tags   = { Name = "${var.environment}-igw" }
}

resource "aws_subnet" "public" {
  count                   = 2
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.${count.index}.0/24"
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true
  tags = { Name = "${var.environment}-public-${count.index + 1}" }
}

resource "aws_subnet" "private" {
  count             = 2
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.${count.index + 10}.0/24"
  availability_zone = data.aws_availability_zones.available.names[count.index]
  tags              = { Name = "${var.environment}-private-${count.index + 1}" }
}

resource "aws_eip" "nat" {
  count  = 2
  domain = "vpc"
}

resource "aws_nat_gateway" "main" {
  count         = 2
  allocation_id = aws_eip.nat[count.index].id
  subnet_id     = aws_subnet.public[count.index].id
  tags          = { Name = "${var.environment}-nat-${count.index + 1}" }
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  tags = { Name = "${var.environment}-public-rt" }
}

resource "aws_route_table" "private" {
  count  = 2
  vpc_id = aws_vpc.main.id
  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main[count.index].id
  }
  tags = { Name = "${var.environment}-private-rt-${count.index + 1}" }
}

resource "aws_route_table_association" "public" {
  count          = 2
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

resource "aws_route_table_association" "private" {
  count          = 2
  subnet_id      = aws_subnet.private[count.index].id
  route_table_id = aws_route_table.private[count.index].id
}

variable "environment" { type = string }

data "aws_availability_zones" "available" { state = "available" }
```

---

**Output do próximo subproblema (Subproblema 2 — IAM):**

```hcl
# Role do cluster EKS
resource "aws_iam_role" "eks_cluster" {
  name = "${var.environment}-eks-cluster-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{ Effect = "Allow"
      Principal = { Service = "eks.amazonaws.com" }
      Action    = "sts:AssumeRole" }]
  })
}

resource "aws_iam_role_policy_attachment" "eks_cluster_policy" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
  role       = aws_iam_role.eks_cluster.name
}

# Role dos nodes EKS
resource "aws_iam_role" "eks_node" {
  name = "${var.environment}-eks-node-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{ Effect = "Allow"
      Principal = { Service = "ec2.amazonaws.com" }
      Action    = "sts:AssumeRole" }]
  })
}

resource "aws_iam_role_policy_attachment" "eks_worker_node" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
  role       = aws_iam_role.eks_node.name
}

resource "aws_iam_role_policy_attachment" "eks_cni" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
  role       = aws_iam_role.eks_node.name
}

resource "aws_iam_role_policy_attachment" "eks_ecr" {
  policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
  role       = aws_iam_role.eks_node.name
}
```

#### O que funcionou

- "Retorne apenas a lista" funcionou: o modelo não tentou implementar nada na decomposição
- As dependências explícitas entre subproblemas guiaram a ordem de resolução naturalmente
- O contexto acumulado no Subproblema 2 eliminou a necessidade de repetir a descrição da infra
- O Terraform saiu parametrizado por `var.environment` — reutilizável para staging e produção

#### O que não funcionou

- O prompt "Resolva o próximo subproblema pendente" é ambíguo se a janela de contexto for perdida ou em nova sessão
- Não há mecanismo de rastreamento automático de quais subproblemas já foram resolvidos

#### Ajustes que eu fiz

- No Subproblema 1 adicionei contexto da infra completa novamente (o prompt de resolução pede isso explicitamente), mesmo já estando na janela da decomposição

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** A decomposição foi excelente e o código Terraform saiu reutilizável. O ponto fraco é que a técnica depende da continuidade da mesma janela de contexto — em sessões longas ou interrompidas, o rastreamento se perde.

#### O que eu faria diferente

Após a decomposição, pediria ao modelo que numerasse os subproblemas em formato de checklist com `[x]` para resolvidos e `[ ]` para pendentes. Isso tornaria o progresso explícito e recuperável mesmo após interrupção.

---

### Experimento 04 — Least-to-Most: Decomposição por Milestones

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

#### Contexto

Variação da Demo 1 com formato de milestones e sub-tarefas. Gera uma estrutura mais próxima de um plano de projeto do que de uma lista técnica de recursos. A execução progressiva usa o prompt minimalista "Execute a próxima tarefa".

#### Prompt usado

**Decomposição em milestones:**
```
Preciso criar a infraestrutura na AWS para uma aplicação web com a
seguinte arquitetura: API em containers rodando no EKS, banco PostgreSQL
gerenciado no RDS, cache Redis no ElastiCache e API Gateway na frente.
A infra precisa estar em multi-AZ, com ambientes de staging e produção
separados.

Crie uma lista de milestones com tarefas e sub-tarefas para a criação
dessa infraestrutura. Organize por ordem de execução, considerando
dependências entre os milestones. Retorne apenas a lista.
```

**Execução progressiva:**
```
Execute a próxima tarefa.
```

#### Resposta da IA

**Milestones:** 8 milestones ordenados por dependência, com sub-tarefas específicas por milestone. Estrutura de projeto mais legível que a lista de subproblemas — útil para apresentar a stakeholders não-técnicos.

**Execução:** O modelo expandiu o Milestone 1 com Terraform completo para VPC, mantendo a consistência com a estrutura declarada na decomposição.

#### Output completo da IA

**Output dos Milestones:**

**Milestone 1 — Networking (Fundação)**
- Criar VPC com CIDR /16
- Configurar subnets públicas e privadas em 2 AZs
- Internet Gateway + 2 NAT Gateways (1 por AZ para HA)
- Route tables e associações

**Milestone 2 — Segurança e Acesso**
- Criar roles IAM para EKS (cluster, nodes), RDS, ElastiCache
- Configurar security groups por componente
- Habilitar VPC Flow Logs

**Milestone 3 — Banco de Dados**
- Criar RDS PostgreSQL multi-AZ em staging
- Criar RDS PostgreSQL multi-AZ em produção
- Configurar parameter groups, backups automatizados e janelas de manutenção
- Validar conectividade a partir da VPC

**Milestone 4 — Cache Redis**
- Criar ElastiCache Redis cluster (staging)
- Criar ElastiCache Redis cluster (produção)
- Validar conectividade

**Milestone 5 — EKS**
- Criar cluster EKS (staging): control plane + managed node groups multi-AZ
- Instalar add-ons: CoreDNS, VPC CNI, kube-proxy
- Criar cluster EKS (produção)
- Configurar OIDC provider para IRSA

**Milestone 6 — API Gateway e Load Balancer**
- Configurar ALB Ingress Controller no EKS
- Criar API Gateway com VPC Link
- Configurar stages (staging/prod), throttling e logging no CloudWatch

**Milestone 7 — Observabilidade**
- CloudWatch dashboards para EKS, RDS, ElastiCache e API Gateway
- Alarmes críticos: CPU, memória, conexões de banco, latência de API
- Configurar log groups e retenção

**Milestone 8 — CI/CD**
- Pipeline de deploy para staging
- Pipeline de deploy para produção com aprovação manual
- Terraform state separado por ambiente (S3 + DynamoDB para lock)

---

**Output da Execução (Milestone 1 expandido):**

```hcl
# Milestone 1 — Networking
# Arquivo: modules/networking/main.tf

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"

  name = "${var.environment}-vpc"
  cidr = "10.0.0.0/16"

  azs             = slice(data.aws_availability_zones.available.names, 0, 2)
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets = ["10.0.11.0/24", "10.0.12.0/24"]

  enable_nat_gateway     = true
  single_nat_gateway     = false
  one_nat_gateway_per_az = true

  enable_dns_hostnames = true
  enable_dns_support   = true

  public_subnet_tags = {
    "kubernetes.io/role/elb" = "1"
  }

  private_subnet_tags = {
    "kubernetes.io/role/internal-elb" = "1"
  }

  tags = { Environment = var.environment }
}

data "aws_availability_zones" "available" { state = "available" }

variable "environment" { type = string }
```

Usando o módulo oficial `terraform-aws-modules/vpc` para seguir boas práticas e aproveitar o suporte nativo a tags Kubernetes necessárias para o ALB Ingress Controller e o EKS.

#### O que funcionou

- O formato de milestones é mais legível e comunicável do que a lista de subproblemas — mais útil para planning de sprint e apresentações
- As tags de subnet Kubernetes (`kubernetes.io/role/elb`) apareceram espontaneamente na execução — boas práticas não pedidas
- O modelo usou o módulo oficial da AWS em vez de recursos nativos avulsos — decisão de qualidade

#### O que não funcionou

- "Execute a próxima tarefa" é ainda mais ambíguo que o formato de subproblemas — sem contexto da janela, o modelo não sabe qual milestone é o próximo
- O modelo usou um módulo externo (`terraform-aws-modules/vpc`) sem avisar que seria necessário `terraform init` com o provider configurado

#### Ajustes que eu fiz

- Nenhum ajuste necessário nos prompts — a resposta já estava no nível esperado

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** O formato de milestones gerou uma estrutura mais rica e pronta para planejamento de projeto. O uso espontâneo de módulos oficiais e tags Kubernetes foi um bônus não pedido.

#### O que eu faria diferente

Especificaria no prompt de execução se quero recursos Terraform nativos ou módulos da comunidade — a escolha implica dependências diferentes e nem sempre é óbvio para quem está aprendendo qual usar.
