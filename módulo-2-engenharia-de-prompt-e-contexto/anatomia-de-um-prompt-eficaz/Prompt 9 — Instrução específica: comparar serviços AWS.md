# Experimento 09 — Instrução específica: comparar serviços AWS

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesmo domínio do Prompt 7 (infraestrutura cloud), com verbo "Compare" e dois objetos específicos (ECS Fargate vs EKS) ancorados num cenário concreto (startup com 3 desenvolvedores). Demonstra como o verbo "Compare" sinaliza ao modelo que a entrega deve ser uma análise de trade-offs, não uma explicação geral.

## Prompt usado

```
Compare ECS Fargate e EKS para rodar microsserviços de uma startup com 3 desenvolvedores.
```

## Resposta da IA

O modelo entregou duas tabelas comparativas (uma por serviço) cobrindo curva de aprendizado, operação, tempo até produção, custo fixo, ecossistema e escalabilidade. Finalizou com uma recomendação direcional fundamentada: ECS Fargate para o cenário dado, com critérios explícitos para quando migrar para EKS (time 8+, multi-cloud, operadores Kubernetes).

## Output completo da IA

Para uma startup com 3 devs, a escolha entre ECS Fargate e EKS envolve trade-offs reais de complexidade, custo e velocidade de entrega:

**ECS Fargate — recomendado para o cenário descrito**

| Critério | ECS Fargate |
|---|---|
| Curva de aprendizado | Baixa — conceitos simples (Task, Service, Cluster) |
| Operação | AWS gerencia o plano de controle e os nodes |
| Tempo até produção | 1-2 dias para primeiro serviço rodando |
| Custo fixo | Zero — paga apenas pelo vCPU/memória alocados |
| Ecosistema | Integração nativa com ALB, CloudWatch, ECR, IAM |
| Escalabilidade | Horizontal via desired count; vertical via task definition |

**EKS — quando faz sentido**

| Critério | EKS |
|---|---|
| Curva de aprendizado | Alta — Kubernetes exige conhecimento de Pods, Services, Ingress, RBAC, etc. |
| Operação | AWS gerencia o control plane, mas nodes e add-ons são sua responsabilidade |
| Tempo até produção | 1-2 semanas para configuração inicial segura |
| Custo fixo | $0,10/hora pelo control plane (~$72/mês) independente do uso |
| Ecosistema | Helm charts, operadores, portabilidade multi-cloud |
| Escalabilidade | Karpenter, HPA, VPA, KEDA — muito mais granular |

**Recomendação para 3 devs:**
Comece com ECS Fargate. O custo cognitivo do Kubernetes consome capacidade de engenharia que uma startup de 3 pessoas não tem. Migre para EKS quando tiver: time de 8+ pessoas, necessidade de portabilidade multi-cloud ou funcionalidades específicas do ecossistema Kubernetes (operadores, CRDs, service mesh).

## O que funcionou

- Verbo "Compare" produziu uma análise de trade-offs em vez de uma descrição genérica de cada serviço
- O cenário concreto (startup, 3 devs) ancou a recomendação final — o modelo não ficou neutro
- Custo fixo do EKS ($72/mês pelo control plane) é um dado concreto relevante para uma startup
- Critérios de migração explícitos dão ao leitor um gatilho de decisão futuro

## O que não funcionou

- Não cobriu custos de transferência de dados entre serviços, que podem ser relevantes dependendo do volume
- Poderia ter mencionado ECS com EC2 como terceira opção intermediária

## Ajustes que eu fiz

- Nenhum — o verbo "Compare" mais o cenário específico foram suficientes para uma análise direcional

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O verbo "Compare" + cenário específico forçaram o modelo a tomar uma posição fundamentada. Resultado é diretamente utilizável numa decisão de arquitetura, com critérios explícitos e recomendação clara.

## O que eu faria diferente

Adicionar o orçamento mensal disponível para infraestrutura e as linguagens da stack atual, pois esses dados mudariam o peso dos critérios de custo e da curva de aprendizado na recomendação.
