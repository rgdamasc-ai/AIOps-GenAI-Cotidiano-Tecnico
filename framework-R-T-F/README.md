## Prompt 1 — E-mail profissional sem estrutura

**Cenário:** Pedido genérico de e-mail solicitando aumento de orçamento. Resultado vem sem tom adequado, sem argumentos e sem estrutura clara.

```
Me escreve um e-mail pedindo aumento de orçamento
```

## Prompt 2 — E-mail profissional com R-T-F

**Cenário:** Mesmo pedido reestruturado com Role (gerente de projetos), Task (e-mail solicitando aumento de 30%) e Format (estrutura do e-mail com limite de palavras). Resultado é um e-mail profissional argumentado.

```markdown
# Role
Você é um gerente de projetos experiente que sabe comunicar decisões de forma objetiva e persuasiva.

# Task
Escreva um e-mail para o diretor de TI solicitando aumento de 30% no orçamento do projeto de migração cloud, justificando com base em escopo ampliado e riscos de atraso.

# Format
E-mail profissional com: linha de assunto, saudação, 3 parágrafos (contexto, justificativa com dados, próximos passos) e encerramento. Máximo 200 palavras.
```

## Prompt 3 — Script de backup PostgreSQL com R-T-F

**Cenário:** Pedido de script Bash de backup automático para PostgreSQL com compactação, rotação de 7 dias e log. Format garante script comentado e organizado.

```markdown
# Role
Você é um DBA senior especializado em PostgreSQL e automação de rotinas operacionais.

# Task
Crie um script Bash de backup automático para um banco PostgreSQL, incluindo compactação com gzip, rotação de backups antigos (manter últimos 7 dias) e log de execução.

# Format
Script Bash comentado, com cabeçalho explicando uso e variáveis configuráveis no topo do arquivo.
```

## Prompt 4 — Documentação técnica com R-T-F

**Cenário:** Pedido de documentação de API Gateway na AWS seguindo template específico. Format define seções, formatos de tabela e estrutura de troubleshooting.

```markdown
# Role
Você é um technical writer especializado em documentação de infraestrutura cloud.

# Task
Crie a documentação de um serviço de API Gateway na AWS, cobrindo: visão geral, arquitetura, endpoints, autenticação e troubleshooting básico.

# Format
Documento Markdown com as seções: Visão Geral, Arquitetura (com descrição textual), Endpoints (tabela com Método | Path | Descrição), Autenticação, Troubleshooting (lista numerada de problemas comuns e soluções).
```
