# Experimento 15 — Demo desestruturada: planejamento de viagem

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Pedido genérico de planejamento de viagem sem nenhum dos 4 componentes da anatomia de prompt (Role, Contexto, Instrução, Output). Primeiro cenário do grupo "Demo" — demonstra o estado base antes de aplicar a estrutura completa.

## Prompt usado

```
Me ajuda a planejar uma viagem
```

## Resposta da IA

O modelo retornou 6 categorias genéricas de dicas de viagem (destino, orçamento, documentação, hospedagem, roteiro, seguro) em texto corrido, sem personalização para nenhum perfil específico. Finalizou com uma sequência de perguntas de clarificação: destino, orçamento e dias disponíveis.

## Output completo da IA

Claro! Planejar uma viagem envolve vários fatores. Aqui vão algumas dicas gerais:

**Destino:** Pense no tipo de experiência que você quer — praia, montanha, cidade histórica, aventura ou cultura. Pesquise o clima da época que pretende viajar.

**Orçamento:** Defina quanto pode gastar no total e divida entre passagem, hospedagem, alimentação e passeios. Plataformas como Google Flights, Kayak e Booking ajudam a comparar preços.

**Documentação:** Verifique se precisa de visto para o destino escolhido. Passaporte válido é indispensável para viagens internacionais.

**Hospedagem:** Hotéis, pousadas, Airbnb ou hostels — cada opção tem custo e experiência diferentes. Reserve com antecedência em alta temporada.

**Roteiro:** Não tente fazer tudo — priorize o que é mais importante para você. Deixe tempo livre para imprevistos e descobertas espontâneas.

**Seguro viagem:** Recomendado especialmente para viagens internacionais. Cobre emergências médicas e cancelamentos.

Me conta mais sobre a sua viagem! Para onde você quer ir? Qual é o seu orçamento e quantos dias tem disponíveis?

## O que funcionou

- O modelo não inventou um destino ou orçamento — reconheceu a falta de contexto e pediu as informações necessárias
- As categorias cobertas são válidas como checklist de planejamento inicial

## O que não funcionou

- Dicas são aplicáveis a qualquer viajante, logo não são úteis para nenhum em particular
- "Não tente fazer tudo" e "deixe tempo para imprevistos" são conselhos genéricos que não ajudam na tomada de decisão real
- O resultado exige pelo menos mais uma rodada de conversa antes de ter qualquer valor concreto
- Sem roteiro, sem destino, sem custo estimado — zero entregável na primeira resposta

## Ajustes que eu fiz

- Nenhum — este é o prompt base sem estrutura, referência direta para o Prompt 16

## Nota (1 a 5)

**Nota:** 1/5

**Justificativa:** A resposta não entrega nada acionável na primeira rodada. Toda a informação necessária para um roteiro real estava disponível (se fosse fornecida no prompt) — o modelo apenas não a tinha. É um exemplo claro de que a qualidade da resposta é proporcional à qualidade do prompt.

## O que eu faria diferente

Aplicar os 4 componentes antes de enviar: Role (agente de viagens especializado), Contexto (perfil do viajante, orçamento, datas, preferências), Instrução (monte um roteiro dia a dia) e Output (tabela Markdown com colunas definidas) — exatamente o que o Prompt 16 demonstra.
