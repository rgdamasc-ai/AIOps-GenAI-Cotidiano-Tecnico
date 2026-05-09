# Experimento 04 — Documentação técnica com R-T-F

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Solicitação de documentação de um serviço API Gateway na AWS. O Role posiciona a IA como technical writer especializado em cloud, o Task define as seções obrigatórias e o Format especifica o uso de Markdown, tabela de endpoints e lista numerada de troubleshooting.

## Prompt usado

```markdown
# Role
Você é um technical writer especializado em documentação de infraestrutura cloud.

# Task
Crie a documentação de um serviço de API Gateway na AWS, cobrindo: visão geral, arquitetura, endpoints, autenticação e troubleshooting básico.

# Format
Documento Markdown com as seções: Visão Geral, Arquitetura (com descrição textual), Endpoints (tabela com Método | Path | Descrição), Autenticação, Troubleshooting (lista numerada de problemas comuns e soluções).
```

## Resposta da IA

A IA gerou um documento Markdown completo, seguindo exatamente a estrutura definida no Format. Todas as cinco seções foram entregues: visão geral objetiva, arquitetura descrita em prosa com fluxo de componentes AWS, tabela de endpoints com 5 exemplos RESTful, bloco de autenticação com JWT e Lambda Authorizer, e troubleshooting com 5 erros comuns e soluções acionáveis.

## Output completo da IA

```markdown
# API Gateway — AWS

## Visão Geral

O API Gateway é o ponto de entrada centralizado para todas as chamadas de API da plataforma, responsável por rotear requisições HTTP/HTTPS para os serviços de backend (Lambda, ECS, EC2) com autenticação, controle de taxa e monitoramento integrados.

## Arquitetura

O fluxo de uma requisição segue o caminho: Cliente → Route 53 (DNS) → API Gateway (estágio de produção) → Integração (Lambda ou backend HTTP). O API Gateway opera em modo Regional, com WAF acoplado para filtragem de tráfego malicioso. Os logs de acesso são enviados ao CloudWatch Logs e as métricas ao CloudWatch Metrics.

## Endpoints

| Método | Path | Descrição |
|--------|------|-----------|
| GET | /v1/health | Health check do serviço |
| GET | /v1/usuarios/{id} | Retorna dados de um usuário |
| POST | /v1/usuarios | Cria novo usuário |
| PUT | /v1/usuarios/{id} | Atualiza dados de usuário |
| DELETE | /v1/usuarios/{id} | Remove usuário |

## Autenticação

Todas as rotas (exceto `/v1/health`) requerem token JWT no header `Authorization: Bearer <token>`. O API Gateway valida o token via Lambda Authorizer integrado ao Cognito User Pool. Tokens têm validade de 1 hora.

## Troubleshooting

1. **403 Forbidden** — Token ausente, expirado ou mal formatado. Verifique o header `Authorization` e renove o token via Cognito.
2. **429 Too Many Requests** — Limite de requisições atingido (1000 req/s por default). Implemente retry com backoff exponencial ou solicite aumento de quota.
3. **502 Bad Gateway** — Erro na integração com o backend (Lambda timeout ou erro não tratado). Verifique os logs do CloudWatch da função Lambda correspondente.
4. **504 Gateway Timeout** — O backend excedeu o tempo limite de 29 segundos. Otimize a função Lambda ou ajuste o timeout da integração.
5. **Endpoint não encontrado (404)** — Verifique se o estágio foi publicado após o último deploy e se o path está correto (incluindo versão `/v1/`).
```

## O que funcionou

- Todas as cinco seções foram entregues na ordem e formato exatos definidos no Format
- A tabela de endpoints usa verbos HTTP corretos e nomenclatura RESTful coerente
- A seção de troubleshooting é acionável: cada erro tem causa e solução específicas
- O Role de technical writer trouxe o limite de 29 segundos do API Gateway — dado técnico preciso que um prompt genérico não entregaria
- Documento pronto para commit em repositório de documentação sem edição

## O que não funcionou

- Endpoints são exemplos genéricos (usuários); em um projeto real precisariam ser substituídos pelos paths reais do serviço
- A seção de Arquitetura ficou em prosa — o Format não pediu diagrama, então a IA não gerou. Se um diagrama Mermaid fosse necessário, deveria estar especificado no Format
- Sem exemplos de request/response body nos endpoints — útil para documentação de API completa

## Ajustes que eu fiz

- Nenhum ajuste no prompt foi necessário
- Para documentação de produção, o Task incluiria os endpoints reais do serviço e o Format pediria exemplos de payload em cada endpoint

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O documento entregue é profissional, tecnicamente preciso e estruturalmente correto. O Format funcionou como um contrato de entrega: a IA seguiu cada requisito sem omissões. O Role de technical writer cloud trouxe especificidades técnicas da AWS (timeout de 29s, WAF, CloudWatch) que um prompt genérico não entregaria.

## O que eu faria diferente

Adicionaria ao Format a solicitação de exemplos de request/response em JSON para cada endpoint e um diagrama de arquitetura em Mermaid. Ambos aumentariam significativamente o valor da documentação para times de desenvolvimento sem custo adicional de prompts.
