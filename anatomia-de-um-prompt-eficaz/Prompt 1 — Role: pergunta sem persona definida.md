# Experimento 01 — Role: pergunta sem persona definida

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Prompt ambíguo sobre "manga" enviado sem nenhum role definido. Objetivo é demonstrar como o modelo mistura significados (fruta, peça de roupa, mangá) quando não sabe qual perspectiva adotar.

## Prompt usado

```
Me fale sobre manga
```

## Resposta da IA

O modelo retornou três blocos distintos cobrindo fruta tropical (variedades, vitaminas, consumo), peça de roupa (estilos de manga: curta, longa, raglan) e mangá japonês (gêneros shonen/shojo/seinen, autores Toriyama e Oda). Finalizou com uma pergunta de clarificação: "Qual das três você quer saber mais?"

## Output completo da IA

A manga pode se referir a várias coisas diferentes:

**Fruta tropical:**
A manga (Mangifera indica) é uma fruta tropical originária do sul da Ásia. É apreciada pelo sabor doce e pela polpa suculenta. As variedades mais comuns no Brasil incluem a Tommy Atkins, Palmer e Espada. Rica em vitamina C, A e fibras, a manga é consumida in natura, em sucos, sorvetes e doces. A safra principal ocorre entre outubro e janeiro nas regiões Norte e Nordeste.

**Peça de roupa:**
A manga é a parte da roupa que cobre o braço. Pode ser longa, curta, três-quartos, bufante, raglan, entre outros estilos. O tipo de manga influencia diretamente o caimento e o conforto da peça.

**Mangá (cultura japonesa):**
Mangá é o termo para histórias em quadrinhos japonesas. Com uma indústria bilionária, os mangás abrangem gêneros como shonen (voltado para jovens), shojo (para meninas), seinen (para adultos) e isekai (mundos fantásticos). Autores como Akira Toriyama (Dragon Ball) e Eiichiro Oda (One Piece) são ícones do gênero.

Qual das três você quer saber mais?

## O que funcionou

- O modelo reconheceu a ambiguidade e cobriu os três significados
- Perguntou ao usuário qual contexto queria aprofundar (boa prática defensiva)

## O que não funcionou

- A resposta é dispersa e não serve diretamente para nenhum caso de uso prático
- Sem foco, nenhum dos três temas foi tratado com profundidade útil
- Quem precisava de uma informação específica recebe ruído nos outros dois blocos

## Ajustes que eu fiz

- Nenhum — este é o prompt base sem modificações, usado como referência para comparação com os Prompts 2, 3 e 4

## Nota (1 a 5)

**Nota:** 2/5

**Justificativa:** A resposta é tecnicamente correta mas operacionalmente inútil — ao tentar cobrir tudo, não aprofunda nada. A pergunta de clarificação ao final indica que o próprio modelo percebe a falta de direção.

## O que eu faria diferente

Definir um role antes da pergunta. Como demonstrado nos Prompts 2, 3 e 4, uma única linha de persona (alfaiate, hortifruti, otaku) elimina toda a ambiguidade e entrega uma resposta focada e acionável.
