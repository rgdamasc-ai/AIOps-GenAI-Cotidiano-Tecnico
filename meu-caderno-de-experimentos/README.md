| Data | Experimento | Categoria | Nota |
|---|---|---|---|
| 2026-05-08 | POD Crashloop | Troubleshooting | 5/5 |
| 2026-05-08 | Conferir quais nodes do minikube estão com crashloop | Automação | 5/5 |
| 2026-05-08 | Rdis yaml| Revisão | 4/5 |

# Aula 06 — Mãos à Obra: Praticando com o que Você Sabe Agora

Material complementar com os três desafios práticos, o template do caderno de experimentos e orientações para você registrar sua baseline antes de entrar no Módulo 02 (Engenharia de Prompt).

## Por que fazer esses exercícios agora

Você terminou o capítulo sabendo que a IA ajuda em análise de logs, scripts, configurações, arquitetura, code review e documentação. Ainda não viu técnicas formais de Engenharia de Prompt (isso vem no próximo módulo). É exatamente esse o ponto: queremos registrar como você usa a IA de forma intuitiva hoje, para que você veja com clareza o quanto evolui depois.

Guarde seus registros. No Módulo 02 você vai revisitar esses mesmos problemas aplicando técnicas estruturadas, e a comparação vai ser o melhor termômetro do seu aprendizado.

## Os três desafios

Escolha tarefas reais do seu ambiente. Não precisa ser nada grandioso, o importante é que sejam problemas seus.

### Exercício 1 — Troubleshooting

Pegue um log, stack trace ou mensagem de erro real que você encontrou nas últimas semanas e peça ajuda à IA para entender e corrigir.

**Exemplos de ponto de partida:**

- Um pod em CrashLoopBackOff no Kubernetes
- Um deploy que falhou em staging
- Um erro de permissão em um pipeline CI/CD
- Um timeout de conexão em um serviço

### Exercício 2 — Automação

Descreva uma tarefa manual repetitiva do seu dia a dia e peça um script ou comando para automatizar.

**Exemplos:**

- Rotacionar logs antigos de um diretório
- Conferir quais nodes do cluster estão com mais de 80% de CPU
- Listar todos os buckets S3 que estão sem criptografia
- Gerar relatório mensal de custos de um recurso específico

### Exercício 3 — Revisão

Cole um trecho de código, configuração, Dockerfile ou manifest seu e peça uma revisão crítica.

**Exemplos:**

- Um Dockerfile que você escreveu recentemente
- Um módulo Terraform
- Uma função de uma API interna
- Um GitHub Actions workflow

## Estrutura sugerida do repositório

```
meu-caderno-de-experimentos/
├── README.md
├── 2026-04-20-troubleshooting-pod-crashloop.md
├── 2026-04-21-automacao-limpeza-logs.md
├── 2026-04-22-revisao-dockerfile-api.md
└── ...
```

O `README.md` pode ser uma tabela simples para você ter visão geral:

```markdown
| Data | Experimento | Categoria | Nota |
|---|---|---|---|
| 2026-04-20 | Pod em CrashLoop | Troubleshooting | 3/5 |
| 2026-04-21 | Limpeza de logs | Automação | 4/5 |
| 2026-04-22 | Revisão Dockerfile | Revisão | 5/5 |
```

## Boas práticas para os registros

- **Nunca cole dados sensíveis.** Anonimize nomes de clientes, chaves, tokens, IPs internos, endpoints privados e qualquer identificador da sua empresa.
- **Versione os prompts.** Se você reformular o prompt três vezes até chegar numa resposta útil, salve as três versões. O valor está justamente em ver a evolução.
- **Marque o modelo e a data.** Modelos mudam, comportamento muda. Saber quando foi e com qual modelo ajuda você a interpretar os resultados depois.
- **Anote hipóteses.** Se você percebeu que "dar um exemplo no prompt melhorou a resposta", registre. Esse é o germe das técnicas que você vai formalizar no próximo módulo.

## O que levar para o Módulo 02

Depois de fazer pelo menos os três exercícios, você terá material suficiente para responder a estas perguntas. Separe-as num arquivo à parte, elas vão ser o seu ponto de partida no módulo seguinte.

- Qual exercício a IA acertou de primeira? Por que você acha que deu certo?
- Em qual exercício você teve que reformular mais vezes? O que estava faltando no seu prompt?
- Qual tipo de resposta a IA costuma entregar bem e qual costuma entregar superficial?
- Se você pudesse aprender uma única técnica para melhorar seus prompts, o que você gostaria que fosse?

Guarde o arquivo. A gente volta nele.

## Encerramento

Mãos à obra. O próximo módulo começa depois que você tiver pelo menos os três experimentos registrados.
