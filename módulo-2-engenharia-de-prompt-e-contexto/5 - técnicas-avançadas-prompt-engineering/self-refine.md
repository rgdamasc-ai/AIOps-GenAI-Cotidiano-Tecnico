# Self Refine

Os prompts abaixo demonstram o ciclo Self-Refine completo em duas demos: proposta de palestra (CFP) e Dockerfile. Cada ciclo tem 3 passos — gerar → criticar → melhorar — executados na mesma janela de contexto.

A diferença para o Chain of Verification é que aqui o modelo critica e melhora o próprio output sem isolamento. É mais barato e rápido, mas suscetível a viés de confirmação. Use quando o custo do erro é baixo e a velocidade importa.

---

## Demo 1 — Proposta de palestra (Passo 1: gerar a proposta)

Pedido inicial de uma CFP. Estrutura mínima: título, abstract e 3-5 tópicos. Sem critérios de qualidade ainda — o objetivo é ter uma versão 1 para criticar.

```
Escreva uma proposta de palestra (CFP) para submeter num evento
de tecnologia. O tema é "Como IA generativa está mudando o dia
a dia de times de engenharia de plataforma". O formato é talk de
30 minutos. O público são engenheiros de software e DevOps com
nível intermediário a avançado. A proposta precisa ter: título,
abstract e 3 a 5 tópicos que serão abordados.
```

---

## Demo 1 — Proposta de palestra (Passo 2: autocrítica com critérios)

Mesmo chat. O modelo agora avalia o que ele mesmo escreveu, usando 5 critérios com nota de 1 a 5. Sem os critérios explícitos, a autocrítica vira elogio genérico.

```
Critique a proposta de palestra que você gerou. Avalie com base
nos seguintes critérios:

1. Gancho: o título e o início do abstract capturam atenção ou são genéricos?
2. Clareza: quem ler entende exatamente o que vai aprender assistindo?
3. Originalidade: a proposta se diferencia de dezenas de outras sobre IA?
4. Público-alvo: o nível técnico tá calibrado pra engenheiros intermediários/avançados?
5. Formato: os tópicos cabem em 30 minutos sem virar superficial?

Para cada critério, dê uma nota de 1 a 5 e justifique.
Retorne em formato markdown.
```

---

## Demo 1 — Proposta de palestra (Passo 3: versão melhorada)

Terceiro passo do ciclo. Pede a v2 corrigindo apenas o que ficou abaixo de 4 — isso evita mudanças cosméticas em pontos que já estavam bons.

```
Com base na sua autocrítica, gere uma versão melhorada da proposta.
Corrija todos os pontos onde a nota foi menor que 4.
Retorne em formato markdown.
```

---

## Demo 2 — Dockerfile (Passo 1: gerar o Dockerfile)

Cenário técnico. Pedido propositalmente vago em segurança/boas práticas — o ciclo Self-Refine vai expor isso depois.

```
Gere um Dockerfile para uma API Node.js com Express.
A aplicação usa npm, tem um arquivo package.json e o
entrypoint é server.js. Gere pronto para build.
gere apenas o dockerfile
```

---

## Demo 2 — Dockerfile (Passo 2: autocrítica com critérios técnicos)

Mesmo chat. 5 critérios objetivos focados em produção de containers. Os critérios mudam por domínio — aqui são técnicos; na demo da CFP eram editoriais.

```
Critique o Dockerfile que você gerou. Avalie com base
nos seguintes critérios:

1. Segurança: roda como root? Tem usuário não-privilegiado?
2. Tamanho da imagem: a imagem base é a mais enxuta possível?
3. Cache de layers: a ordem dos comandos aproveita o cache do Docker?
4. Multi-stage: separa build de runtime?
5. Boas práticas: tem .dockerignore? Usa COPY em vez de ADD?

Para cada critério, dê uma nota de 1 a 5 e explique o que melhorar.
Retorne apenas a avaliação
```

---

## Demo 2 — Dockerfile (Passo 3: versão melhorada)

Fechamento do ciclo. Mesma instrução da Demo 1 — só corrigir o que ficou abaixo de 4.

```
Com base na sua autocrítica, gere uma versão melhorada do
Dockerfile. Corrija todos os pontos onde a nota foi menor que 4.
```

---

## Caderno de Experimentos

---

### Experimento 07 — Self-Refine: Proposta de Palestra (CFP)

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Revisão

#### Contexto

Ciclo Self-Refine de 3 passos aplicado à criação de uma proposta de palestra (CFP) para evento de tecnologia. O modelo gera a v1, se autocritica com critérios explícitos (1-5) e gera a v2 corrigindo apenas o que ficou abaixo de 4. Sem critérios explícitos, a autocrítica tende a ser genérica.

#### Prompt usado

**Passo 1 — Gerar a proposta:**
```
Escreva uma proposta de palestra (CFP) para submeter num evento
de tecnologia. O tema é "Como IA generativa está mudando o dia
a dia de times de engenharia de plataforma". O formato é talk de
30 minutos. O público são engenheiros de software e DevOps com
nível intermediário a avançado. A proposta precisa ter: título,
abstract e 3 a 5 tópicos que serão abordados.
```

**Passo 2 — Autocrítica com critérios:**
```
Critique a proposta de palestra que você gerou. Avalie com base
nos seguintes critérios:
1. Gancho: o título e o início do abstract capturam atenção ou são genéricos?
2. Clareza: quem ler entende exatamente o que vai aprender assistindo?
3. Originalidade: a proposta se diferencia de dezenas de outras sobre IA?
4. Público-alvo: o nível técnico está calibrado para engenheiros intermediários/avançados?
5. Formato: os tópicos cabem em 30 minutos sem virar superficial?

Para cada critério, dê uma nota de 1 a 5 e justifique.
Retorne em formato markdown.
```

**Passo 3 — Versão melhorada:**
```
Com base na sua autocrítica, gere uma versão melhorada da proposta.
Corrija todos os pontos onde a nota foi menor que 4.
Retorne em formato markdown.
```

#### Resposta da IA

**Passo 1:** Proposta com título genérico ("Como IA Generativa Está Mudando o Dia a Dia..."), abstract funcional e 5 tópicos cobertos de forma abrangente mas sem diferenciação clara.

**Passo 2:** Notas — Gancho: 2/5, Clareza: 4/5, Originalidade: 2/5, Público-alvo: 4/5, Formato: 4/5. A autocrítica identificou corretamente que título e originalidade eram os pontos fracos.

**Passo 3:** Título completamente reformulado, abstract com ângulo específico e único, mantendo os tópicos que já estavam em 4/5.

#### Output completo da IA

**Output do Passo 1 (proposta v1):**

**Título:** Como IA Generativa Está Mudando o Dia a Dia de Times de Engenharia de Plataforma

**Abstract:**
Times de engenharia de plataforma enfrentam um desafio crescente: escalar serviços internos sem escalar o headcount na mesma proporção. A IA generativa está se tornando uma alavanca real para isso — não como hype, mas como parte da rotina de quem mantém pipelines, cuida de incidentes e documenta sistemas. Nesta talk, você vai ver como incorporar ferramentas de IA generativa em workflows reais de plataforma, do onboarding de novos engenheiros até a triagem de alertas em produção.

**Tópicos:**
1. O que mudou: da assistência de código para automação de workflows inteiros
2. Casos reais de uso em plataforma: triagem de incidentes, geração de runbooks, code review de IaC
3. Como integrar IA no fluxo sem criar dependência opaca ("por que isso funcionou?")
4. Prompt engineering para contextos técnicos: como obter respostas úteis de sistemas complexos
5. Armadilhas comuns: quando IA piora o fluxo e como identificar esses casos

---

**Output do Passo 2 (autocrítica):**

| Critério | Nota | Justificativa |
|---|---|---|
| Gancho | 2/5 | Título genérico — o formato "Como X está mudando Y" é o mais usado em talks de IA do momento. O início do abstract começa com um problema de headcount, funcional mas sem impacto. Não diferencia nada. |
| Clareza | 4/5 | Quem ler entende o que vai aprender. Os 5 tópicos são específicos o suficiente para criar expectativa concreta. A palavra "rotina" ancora bem o tom prático. |
| Originalidade | 2/5 | "IA no dia a dia de plataforma" está em toda conferência de tech em 2024-2025. Falta um ângulo que só este speaker poderia dar — uma experiência específica, um dado, um fracasso. |
| Público-alvo | 4/5 | Tópicos como triagem de incidentes, runbooks e IaC são específicos para o público intermediário/avançado. Não há conteúdo 101. |
| Formato | 4/5 | 5 tópicos para 30 minutos é plausível se cada um for tratado com objetividade. O tópico 3 pode ser comprimido para dar mais espaço a demos. |

---

**Output do Passo 3 (proposta v2 — corrigindo notas < 4):**

**Título:** Automatizei o onboarding do meu time com IA — e o que aprendi depois de 3 meses

**Abstract:**
Engenheiros de plataforma passam boa parte do tempo mantendo o que os outros constroem. O problema: esse conhecimento — como o sistema falha, por que aquela config existe, o que aquele alerta realmente significa — fica na cabeça de 2 ou 3 pessoas. A IA generativa não resolve isso magicamente. Mas com a abordagem certa, ela vira um multiplicador de conhecimento real: triagens mais rápidas, runbooks que se escrevem quase sozinhos, code review de IaC que encontra o que o olho humano cansado perde. Nesta talk, você vai ver exatamente como — com exemplos reais de um time que errou primeiro e ajustou depois.

**Tópicos** (mantidos, já em 4/5):
1. O que IA resolve bem em plataforma — e o que ela não resolve
2. Triagem de incidentes com LLMs: o que muda, o que ainda depende de você
3. Runbooks gerados por IA: como transformar histórico de incidentes em documentação útil
4. Code review de IaC com IA: os casos onde ela encontrou o que eu não vi
5. O que deu errado: dependência opaca, alucinações em contexto técnico e como contornar

#### O que funcionou

- A autocrítica com critérios numéricos (1-5) produziu análise específica e acionável — sem os critérios, o modelo teria se auto-elogiado
- A instrução "corrija apenas o que ficou abaixo de 4" foi respeitada — o modelo não tocou nos tópicos (nota 4) nem na clareza (nota 4)
- O título v2 é completamente diferente e muito mais específico — narrativa de experiência real vs. título genérico

#### O que não funcionou

- O modelo deu nota 2 para Originalidade mas não sugeriu uma abordagem específica no Passo 2 — só no Passo 3 ficou claro que a mudança era de tom genérico para narrativa pessoal
- O abstract v2 ficou mais longo que o ideal para um CFP (normalmente 150-200 palavras)

#### Ajustes que eu fiz

- Nenhum ajuste nos prompts. O ciclo de 3 passos foi suficiente.

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** O Self-Refine produziu uma melhoria real e mensurável no output. O único ponto fraco é que o modelo não é completamente isento de viés sobre o próprio trabalho — as notas 4 podem estar ligeiramente infladas.

#### O que eu faria diferente

Rodaria um segundo ciclo de Self-Refine na v2, com critérios diferentes (brevidade do abstract, aderência ao formato do evento específico, call-to-action para o organizador). Dois ciclos com critérios diferentes cobrem mais dimensões que um ciclo com muitos critérios.

---

### Experimento 08 — Self-Refine: Dockerfile

**Data:** 2026-05-24
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Revisão

#### Contexto

Ciclo Self-Refine aplicado a um Dockerfile para API Node.js/Express. O prompt inicial é propositalmente vago em segurança e boas práticas — o ciclo revela as omissões via autocrítica com critérios técnicos específicos de containers em produção.

#### Prompt usado

**Passo 1:**
```
Gere um Dockerfile para uma API Node.js com Express.
A aplicação usa npm, tem um arquivo package.json e o
entrypoint é server.js. Gere pronto para build.
gere apenas o dockerfile
```

**Passo 2:**
```
Critique o Dockerfile que você gerou. Avalie com base
nos seguintes critérios:
1. Segurança: roda como root? Tem usuário não-privilegiado?
2. Tamanho da imagem: a imagem base é a mais enxuta possível?
3. Cache de layers: a ordem dos comandos aproveita o cache do Docker?
4. Multi-stage: separa build de runtime?
5. Boas práticas: tem .dockerignore? Usa COPY em vez de ADD?

Para cada critério, dê uma nota de 1 a 5 e explique o que melhorar.
Retorne apenas a avaliação
```

**Passo 3:**
```
Com base na sua autocrítica, gere uma versão melhorada do
Dockerfile. Corrija todos os pontos onde a nota foi menor que 4.
```

#### Resposta da IA

**Passo 1:** Dockerfile básico com `node:18`, sem usuário não-privilegiado, sem multi-stage e sem aproveitar cache de layers corretamente.

**Passo 2:** Notas — Segurança: 1/5, Tamanho: 2/5, Cache: 3/5, Multi-stage: 1/5, Boas práticas: 3/5. Quatro dos cinco critérios abaixo de 4.

**Passo 3:** Dockerfile reescrito com multi-stage (builder + runtime), `node:18-alpine`, usuário não-privilegiado, `npm ci --only=production` e ordem de layers otimizada.

#### Output completo da IA

**Output do Passo 1 (Dockerfile v1):**

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

---

**Output do Passo 2 (autocrítica):**

| Critério | Nota | O que melhorar |
|---|---|---|
| Segurança | 1/5 | Roda como root (padrão da imagem node:18). Qualquer processo comprometido tem acesso root ao container. Criar usuário não-privilegiado. |
| Tamanho da imagem | 2/5 | `node:18` é a imagem completa Debian (~1GB). `node:18-alpine` reduziria para ~180MB. |
| Cache de layers | 3/5 | A ordem está parcialmente correta (package.json antes do código). Mas `npm install` instala devDependencies desnecessárias em produção. Usar `npm ci --only=production`. |
| Multi-stage | 1/5 | Sem multi-stage build. Build e runtime estão na mesma imagem, incluindo ferramentas de build desnecessárias. |
| Boas práticas | 3/5 | Usa COPY corretamente. `npm install` deveria ser `npm ci` para reprodutibilidade. Sem menção a .dockerignore (arquivo separado mas necessário). |

---

**Output do Passo 3 (Dockerfile v2):**

```dockerfile
# Stage 1: instalar dependências (incluindo dev para possível build step)
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

# Stage 2: runtime mínimo
FROM node:18-alpine AS runtime

WORKDIR /app

# Criar usuário não-privilegiado
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Instalar apenas dependências de produção
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

# Copiar código-fonte do stage builder
COPY --from=builder --chown=nodejs:nodejs /app/server.js .

# Usar usuário não-privilegiado
USER nodejs

EXPOSE 3000

CMD ["node", "server.js"]
```

`.dockerignore` recomendado:
```
node_modules
npm-debug.log
.git
.env
*.test.js
```

#### O que funcionou

- Os critérios técnicos produziram autocrítica específica e acionável — sem eles, o modelo teria dito "está bom, poderia adicionar multi-stage"
- O Dockerfile v2 corrigiu todos os pontos abaixo de 4 sem tocar nos que já estavam em 3 (cache de layers já estava parcialmente correto)
- O modelo incluiu o .dockerignore sugerido como bônus, não pedido

#### O que não funcionou

- O modelo não sinalizou que o COPY no stage 2 deve copiar todos os arquivos necessários do projeto (apenas `server.js` pode não ser suficiente se houver estrutura de pastas)
- A nota 3 de "Cache de layers" deveria ter sido 2 — a ordem estava errada para aproveitar cache quando há mudança de código sem mudança de dependências

#### Ajustes que eu fiz

- Nenhum ajuste nos prompts. O ciclo funcionou conforme esperado.

#### Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** O Self-Refine produziu uma melhoria técnica real e substancial. O Dockerfile v2 está production-ready em 4 das 5 dimensões. O ponto fraco é que a nota 3 de "Cache de layers" estava subestimada — a ordem de COPY ainda pode ser melhorada.

#### O que eu faria diferente

No Passo 1 adicionaria contexto sobre a estrutura de pastas da aplicação (ex: tem pasta `src/`, tem `scripts/` de build?) para que o multi-stage do Passo 3 copiasse todos os artefatos necessários, não apenas `server.js`.
