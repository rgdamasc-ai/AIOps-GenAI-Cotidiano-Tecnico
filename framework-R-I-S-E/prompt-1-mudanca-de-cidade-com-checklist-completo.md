# Prompt 1 — Mudança de cidade com checklist completo

**Framework:** R.I.S.E. (Role, Input, Steps, Expectation)

**Cenário:** Planejamento de mudança interestadual de família de 4 pessoas (SP → Florianópolis). Steps definem a sequência (custo de vida, bairros, escolas, custos, cronograma, checklist) e Expectation amarra o formato.

```markdown
# Role
Você é um consultor de relocação com 15 anos de experiência em mudanças interestaduais no Brasil, especializado em famílias com crianças.

# Input
- Família: casal + 2 filhos (8 e 12 anos, em idade escolar)
- Origem: São Paulo/SP (apartamento alugado, 90m²)
- Destino: Florianópolis/SC (bairro a definir)
- Orçamento total para mudança: R$ 15.000
- Prazo: 60 dias a partir de hoje
- Ambos os pais trabalham remoto
- Prioridades: qualidade de vida, boas escolas, custo de vida menor

# Steps
1. Analisar custo de vida comparativo SP vs Florianópolis (moradia, alimentação, transporte, educação)
2. Recomendar 3 bairros em Florianópolis que atendam as prioridades da família
3. Listar escolas particulares e públicas bem avaliadas nos bairros recomendados
4. Estimar custos detalhados da mudança (frete, viagem, depósito do aluguel, primeiros meses)
5. Montar cronograma de 60 dias com marcos semanais
6. Criar checklist de documentação e providências (transferência escolar, endereço, contas)

# Expectation
Espero um plano de ação completo em formato de documento estruturado com:
- Tabela comparativa de custos SP vs Florianópolis
- Ranking dos 3 bairros com prós e contras
- Cronograma visual com marcos semanais e responsáveis
- Checklist final com critério de conclusão para cada item
- Orçamento detalhado que não ultrapasse R$ 15.000
```

---

# Experimento 01 — Planejamento de mudança interestadual SP → Florianópolis

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** claude-sonnet-4-6
**Categoria:** Automação

## Contexto

Planejamento completo de mudança interestadual para família de 4 pessoas (SP → Florianópolis),
com orçamento de R$ 15.000 e prazo de 60 dias. Pais trabalham remoto; prioridades são qualidade
de vida, boas escolas e custo de vida menor.

## Prompt usado

```markdown
# Role
Você é um consultor de relocação com 15 anos de experiência em mudanças interestaduais no
Brasil, especializado em famílias com crianças.

# Input
- Família: casal + 2 filhos (8 e 12 anos, em idade escolar)
- Origem: São Paulo/SP (apartamento alugado, 90m²)
- Destino: Florianópolis/SC (bairro a definir)
- Orçamento total para mudança: R$ 15.000
- Prazo: 60 dias a partir de hoje
- Ambos os pais trabalham remoto
- Prioridades: qualidade de vida, boas escolas, custo de vida menor

# Steps
1. Analisar custo de vida comparativo SP vs Florianópolis
2. Recomendar 3 bairros em Florianópolis que atendam as prioridades da família
3. Listar escolas particulares e públicas bem avaliadas nos bairros recomendados
4. Estimar custos detalhados da mudança
5. Montar cronograma de 60 dias com marcos semanais
6. Criar checklist de documentação e providências

# Expectation
Espero um plano de ação completo em formato de documento estruturado com:
- Tabela comparativa de custos SP vs Florianópolis
- Ranking dos 3 bairros com prós e contras
- Cronograma visual com marcos semanais e responsáveis
- Checklist final com critério de conclusão para cada item
- Orçamento detalhado que não ultrapasse R$ 15.000
```

## Resposta da IA

O modelo entregou um plano de ação completo e estruturado em 6 seções correspondentes aos Steps.
Gerou tabela comparativa de custos (economia estimada de 35%), ranking dos 3 bairros (Lagoa da
Conceição, Campeche, Ingleses) com prós e contras, lista de escolas por bairro, orçamento
detalhado dentro do limite de R$ 15.000, cronograma semanal de 9 semanas e checklist com
critérios de conclusão para cada item. A resposta cobriu todos os entregáveis exigidos no
Expectation sem necessidade de reformulação.

## Output completo da IA

# Plano de Relocação: São Paulo → Florianópolis

## 1. Análise Comparativa de Custo de Vida: SP vs Florianópolis

| Categoria | São Paulo/SP | Florianópolis/SC | Economia Estimada |
|---|---|---|---|
| Aluguel (90m², bom padrão) | R$ 4.500–6.000/mês | R$ 2.800–4.000/mês | 30–40% |
| Alimentação (família de 4) | R$ 2.500–3.000/mês | R$ 1.800–2.200/mês | 25–30% |
| Transporte | R$ 800–1.200/mês | R$ 400–600/mês | 50% |
| Escola particular (2 filhos) | R$ 3.000–5.000/mês | R$ 1.500–3.000/mês | 30–50% |
| Plano de saúde (família) | R$ 1.200–1.800/mês | R$ 1.000–1.500/mês | 10–15% |
| **Total estimado** | **R$ 12.000–17.000/mês** | **R$ 7.500–11.300/mês** | **~35%** |

**Conclusão:** A família pode economizar entre R$ 3.000 e R$ 6.000/mês após a mudança,
cobrindo o custo total da relocação em 3–5 meses.

---

## 2. Ranking dos 3 Bairros Recomendados

### 1º — Lagoa da Conceição

**Prós:**
- Qualidade de vida excepcional (praia, lagoa, natureza)
- Comunidade cultural ativa
- Boas opções de comércio e serviços próximos
- Rota de ciclismo segura para crianças

**Contras:**
- Trânsito intenso no verão
- Aluguéis ligeiramente mais altos
- Escola pública do bairro requer atenção

**Perfil ideal:** Família que prioriza qualidade de vida e natureza

---

### 2º — Campeche

**Prós:**
- Excelente custo-benefício de aluguel
- Bairro tranquilo e familiar
- Próximo a boas escolas particulares
- Em crescimento com boa infraestrutura

**Contras:**
- Mais afastado do centro
- Transporte público limitado
- Sem opções culturais noturnas próximas

**Perfil ideal:** Família que quer tranquilidade e custo de vida reduzido

---

### 3º — Ingleses

**Prós:**
- Bairro consolidado com comércio completo
- Escolas bem avaliadas na região
- Acesso à praia e qualidade de vida
- Aluguel competitivo

**Contras:**
- Trânsito complicado no verão
- Mais distante do centro da ilha

**Perfil ideal:** Família com crianças que busca estrutura e custo equilibrado

---

## 3. Escolas por Bairro

### Lagoa da Conceição
**Particulares:** Colégio Energia — Ensino Fundamental I e II, bem avaliado no IDEB
**Públicas:** EB Vitor Miguel de Oliveira (próxima ao bairro)

### Campeche
**Particulares:** Colégio Saber (Ensino Fundamental, metodologia ativa), Escola Alternativa Arco-Íris (pedagogia diferenciada)
**Públicas:** EBM Brigadeiro Eduardo Gomes

### Ingleses
**Particulares:** Colégio Dom Bosco (tradição e bom desempenho), Colégio Geração
**Públicas:** EBM Donícia Maria da Costa

> **Recomendação:** Pesquise as escolas com visita presencial antes de fechar o imóvel.
> Priorize proximidade do novo endereço para qualidade de vida das crianças.

---

## 4. Orçamento Detalhado da Mudança

| Item | Custo Estimado |
|---|---|
| Frete (SP → Florianópolis, 90m² de mobília) | R$ 6.500–8.000 |
| Viagem (combustível ou passagens, família de 4) | R$ 800–1.500 |
| Hospedagem em trânsito (1–2 noites) | R$ 300–600 |
| Taxa de seguro-fiança ou fiador jurídico | R$ 800–1.200 |
| Pequenos reparos e adaptações no novo imóvel | R$ 500–1.000 |
| Reserva de emergência | R$ 2.000 |
| **Total** | **R$ 10.900–14.300** |

> **Dica crítica:** Negocie o depósito com seguro-fiança em vez de 3 meses adiantados —
> economiza R$ 7.000–10.000 de capital de giro.

---

## 5. Cronograma de 60 Dias com Marcos Semanais

| Semana | Período | Ação | Responsável |
|---|---|---|---|
| S1 | Dias 1–7 | Pesquisa de imóveis online nos 3 bairros; contato com imobiliárias | Ambos |
| S2 | Dias 8–14 | Viagem de reconnaissance (visitar imóveis, bairros, escolas) | Ambos |
| S3 | Dias 15–21 | Assinar contrato; contratar frete; notificar locador SP | Ambos |
| S4 | Dias 22–28 | Matrícula nas novas escolas; solicitar transferência escolar | Responsável 1 |
| S5 | Dias 29–35 | Desfazer contratos SP; contratar serviços em Floripa | Responsável 2 |
| S6 | Dias 36–42 | Embalar e organizar mudança; separar itens essenciais | Ambos |
| S7 | Dias 43–49 | Dia da mudança; viagem; entrega do apartamento SP com vistoria | Ambos |
| S8 | Dias 50–56 | Instalação em Floripa; contratação de serviços locais | Ambos |
| S9 | Dias 57–60 | Regularização de endereço; crianças iniciam escola | Ambos |

---

## 6. Checklist de Documentação e Providências

### Documentação Pessoal
- [ ] Atualizar endereço no CPF (Receita Federal online) — **Critério:** confirmação por email recebida
- [ ] Atualizar CNH (DETRAN-SC) — **Critério:** CNH com endereço SC em mãos
- [ ] Atualizar título de eleitor — **Critério:** transferência confirmada no TRE
- [ ] Atualizar endereço no banco — **Critério:** extrato com novo endereço impresso

### Escola
- [ ] Solicitar declaração de transferência escolar (escola SP) — **Critério:** documento em mãos
- [ ] Entregar documentação na nova escola (Floripa) — **Critério:** matrícula confirmada por escrito
- [ ] Confirmar data de início das aulas — **Critério:** calendário escolar recebido

### Serviços e Utilidades
- [ ] Comunicar mudança aos Correios — **Critério:** redirecionamento ativo
- [ ] Cancelar contratos SP — **Critério:** confirmação de cancelamento
- [ ] Contratar internet em Floripa — **Critério:** instalação confirmada antes do dia da mudança
- [ ] Verificar portabilidade do plano de saúde — **Critério:** carteirinha recebida

### Trabalho Remoto
- [ ] Comunicar mudança de endereço ao empregador — **Critério:** RH confirmou atualização
- [ ] Testar conectividade no novo endereço — **Critério:** videoconferência de teste realizada

## O que funcionou

- O framework R.I.S.E. eliminou a necessidade de iterações: o primeiro output já cobriu todos
  os 6 entregáveis especificados no Expectation
- O elemento Steps forçou sequência lógica (análise → bairros → escolas → custo → cronograma
  → checklist), evitando que o modelo pulasse etapas
- O Expectation com bullets específicos (tabela, ranking, cronograma visual, checklist com
  critério) garantiu formatação acionável sem necessidade de reformulação
- A dica crítica sobre seguro-fiança (economiza R$ 7.000–10.000) surgiu espontaneamente —
  informação prática de alto valor não pedida explicitamente

## O que não funcionou

- Nomes de escolas são genéricos — sem acesso a dados em tempo real, o modelo não pode
  confirmar avaliações IDEB atualizadas; requer verificação manual
- Faixas de preço de aluguel têm variância alta; os valores precisam ser checados em
  plataformas como QuintoAndar ou Viva Real antes de usar
- O cronograma assume que o imóvel em Floripa será encontrado na semana 2 — risco real se
  o mercado estiver aquecido

## Ajustes que eu fiz

- Nenhum: o prompt foi executado uma única vez, sem refinamento adicional

## Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** Entregou todos os entregáveis solicitados em uma única execução, com
formatação consistente e informações práticas. A nota não é 5 pois os dados de escolas e
preços são estimativas não verificáveis sem acesso a fontes externas em tempo real.

## O que eu faria diferente

Adicionar no Input um link ou referência a fontes de dados (ex: QuintoAndar, Numbeo, IDEB)
e pedir ao modelo para indicar quais informações precisam de verificação manual — isso
tornaria o output mais honesto sobre os limites do conhecimento do modelo.
