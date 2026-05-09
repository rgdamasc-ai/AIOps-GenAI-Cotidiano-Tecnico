# Experimento 16 — Demo estruturada: planejamento de viagem com 4 componentes

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma intenção do Prompt 15 (planejar uma viagem), reestruturada com os 4 componentes completos: Role (agente de viagens econômico), Contexto (casal, R$ 5.000, 7 dias em julho, SP, natureza/trilhas), Instrução (roteiro dia a dia) e Output (tabela Markdown com colunas definidas e total ao final). Demonstração central do impacto da estrutura completa.

## Prompt usado

```
# Role
Você é um agente de viagens especializado em roteiros econômicos pela América do Sul.

# Contexto
Casal sem filhos, orçamento de R$ 5.000 total, disponibilidade de 7 dias em julho,
saindo de São Paulo. Preferem natureza e trilhas, não curtem balada.

# Instrução
Monte um roteiro dia a dia com destino, atividades e estimativa de custo.

# Output
Formato: tabela Markdown com colunas Dia | Destino | Atividade | Custo estimado.
Inclua totais ao final. Apenas opções dentro do orçamento.
```

## Resposta da IA

O modelo entregou um roteiro completo de 7 dias com Bonito (MS) + Pantanal como destinos — adequados ao perfil (natureza, trilhas, fauna). A tabela inclui dia a dia com atividade específica e custo estimado por item, mais duas linhas de custo fixo (hospedagem 7 noites + alimentação 7 dias). Total apurado: R$ 4.440, dentro do orçamento de R$ 5.000, com saldo de R$ 560 destacado para imprevistos.

## Output completo da IA

| Dia | Destino | Atividade | Custo estimado |
|---|---|---|---|
| 1 | São Paulo → Bonito (MS) | Voo GRU→Campo Grande + transfer + chegada | R$ 900 (2 passagens) |
| 2 | Bonito (MS) | Flutuação no Rio da Prata (snorkel em água cristalina) | R$ 380 (2 pessoas) |
| 3 | Bonito (MS) | Trilha na Serra da Bodoquena + Buraco das Araras | R$ 180 |
| 4 | Bonito (MS) | Abismo Anhumas (rapel em caverna + mergulho) | R$ 600 (2 pessoas) |
| 5 | Bonito → Pantanal (MS) | Transfer + safári fotográfico de barco | R$ 350 |
| 6 | Pantanal (MS) | Trilha a pé com guia local — fauna nativa (onça, tuiuiú, capivara) | R$ 200 |
| 7 | Pantanal → São Paulo | Transfer + voo de volta | R$ 500 |
| — | **Hospedagem (7 noites pousada simples)** | — | R$ 840 |
| — | **Alimentação (7 dias × R$70/dia)** | — | R$ 490 |
| — | **TOTAL** | — | **R$ 4.440** |

*Saldo restante: R$ 560 para imprevistos, souvenirs ou upgrade de alguma atividade.*

## O que funcionou

- Destino escolhido (Bonito + Pantanal) é coerente com o perfil declarado (natureza, trilhas, sem balada)
- Restrição de orçamento respeitada — total calculado e saldo residual destacado
- Cada dia tem uma atividade específica com custo unitário, não uma categoria genérica
- A tabela está pronta para colar em um e-mail, documento ou aplicativo de planejamento
- Nenhuma pergunta de clarificação — o prompt forneceu todas as variáveis necessárias

## O que não funcionou

- Custos de alimentação (R$ 70/dia) e hospedagem (R$ 120/noite) podem variar significativamente dependendo da pousada e restaurante escolhidos — são estimativas, não preços garantidos
- Não incluiu alertas sobre períodos de chuva no Pantanal em julho (mês de seca, na verdade ótimo, mas vale mencionar)

## Ajustes que eu fiz

- Nenhum — o prompt estruturado entregou o roteiro completo na primeira tentativa, sem reformulação

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Contraste com o Prompt 15 é máximo: de zero entregável para um roteiro completo, dentro do orçamento, com custo por atividade e saldo residual. Os 4 componentes eliminaram todas as rodadas de clarificação e produziram valor direto na primeira resposta.

## O que eu faria diferente

Adicionar uma restrição de mobilidade (ex: "não dirigem, precisam de transfers incluídos") e pedir alternativas caso alguma atividade esteja fechada em julho, tornando o roteiro ainda mais robusto para execução real.
