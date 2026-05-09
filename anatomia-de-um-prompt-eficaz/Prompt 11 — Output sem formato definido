# Experimento 11 — Output sem formato definido

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pedido para listar serviços AWS de containers sem especificação de formato de saída. Primeiro cenário do grupo "Output" — demonstra como a ausência de formato gera texto corrido com mistura de explicações, nuances e exemplos, dificultando o reuso direto da resposta.

## Prompt usado

```
Liste os serviços AWS para hospedar containers.
```

## Resposta da IA

O modelo retornou 6 serviços em texto corrido com parágrafos descritivos: ECS, EKS, Fargate, ECR, App Runner e Lambda Container Images. Cada item misturou definição, caso de uso, vantagens e ressalvas num único bloco narrativo. Sem delimitação clara entre os campos de cada serviço.

## Output completo da IA

A AWS oferece vários serviços para hospedar containers, cada um com características diferentes:

**Amazon ECS (Elastic Container Service)** é o serviço de orquestração nativo da AWS. Pode ser usado com instâncias EC2 (você gerencia os servidores) ou com Fargate (serverless, sem gerenciar servidores). É mais simples de operar do que Kubernetes e tem integração nativa com outros serviços AWS como ALB, IAM e CloudWatch.

**Amazon EKS (Elastic Kubernetes Service)** oferece Kubernetes gerenciado. A AWS cuida do control plane, mas você ainda precisa gerenciar os worker nodes (a menos que use Fargate com EKS). É a melhor opção quando você já tem expertise em Kubernetes ou precisa de portabilidade multi-cloud.

**AWS Fargate** não é exatamente um serviço de container em si — é um modo de execução serverless que funciona tanto com ECS quanto com EKS. Você especifica CPU e memória, e a AWS provisiona a infraestrutura automaticamente. Ideal quando você não quer gerenciar servidores.

**Amazon ECR (Elastic Container Registry)** é o registro privado de imagens Docker da AWS. Não hospeda containers em execução, mas é onde você armazena as imagens que serão usadas pelo ECS, EKS ou outros serviços.

**AWS App Runner** é o serviço mais simples: você aponta para uma imagem de container ou repositório de código e ele cuida de tudo (build, deploy, escalabilidade, HTTPS). Ideal para aplicações web e APIs sem configuração complexa, mas com menos controle sobre a infraestrutura.

**AWS Lambda Container Images** permite rodar containers no Lambda usando imagens Docker de até 10GB. Segue o modelo serverless por invocação, ideal para workloads event-driven ou intermitentes.

## O que funcionou

- Cobertura completa — incluiu ECR e Lambda Container Images que prompts mais restritivos excluiriam
- Nuances importantes apareceram (ex: "Fargate não é um serviço de container em si — é um modo de execução")
- Tom explicativo útil para quem está aprendendo o ecossistema

## O que não funcionou

- Texto corrido é difícil de colar em documentação, wiki ou apresentação
- Não há ordem clara (do mais simples ao mais complexo, por exemplo)
- Campos misturados (definição + caso de uso + ressalva) no mesmo parágrafo dificultam comparação direta
- Para alimentar outro sistema ou gerar uma tabela manualmente, o engenheiro precisaria reprocessar a resposta

## Ajustes que eu fiz

- Nenhum — este é o baseline sem formato, referência para os Prompts 12, 13 e 14

## Nota (1 a 5)

**Nota:** 3/5

**Justificativa:** Informação correta e completa, mas formato inadequado para reuso direto. A ausência de estrutura força o leitor a fazer a triagem e organização que o modelo poderia ter feito com uma instrução de formato.

## O que eu faria diferente

Especificar o formato desejado antes de enviar: tabela Markdown, JSON estruturado ou lista com campos fixos. Como demonstrado nos Prompts 12, 13 e 14, uma linha de instrução de formato transforma a resposta em algo diretamente utilizável.
