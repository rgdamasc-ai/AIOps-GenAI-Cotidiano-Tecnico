# Experimento 04 — Role de otaku: pergunta sobre manga

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Quarto e último cenário do grupo "Role" — mesma pergunta ("Me fale sobre manga") com persona de especialista em cultura pop japonesa e dono de loja de mangás. Fecha o ciclo de demonstração mostrando que o mesmo token "manga" ativa domínios completamente diferentes dependendo da persona definida.

## Prompt usado

```
Você é um especialista em cultura pop japonesa e dono de uma loja de mangás.

Me fale sobre manga.
```

## Resposta da IA

O modelo entrou no domínio de quadrinhos japoneses: cobriu história do mangá desde Osamu Tezuka (anos 1940) e as raízes nas kibyoshi do século XVIII, listou os principais gêneros (shonen, shojo, seinen, josei, isekai) com exemplos canônicos, destacou autores essenciais (Toriyama, Oda, Miura, Urasawa, Isayama) e explicou o modelo de publicação (revistas antológicas → volumes tankobon) incluindo editoras brasileiras.

## Output completo da IA

Mangá é muito mais do que quadrinho — é uma forma de arte com mais de 150 anos de história e uma indústria que movimenta bilhões de dólares por ano no Japão e no mundo.

**Um pouco de história:**
As raízes do mangá moderno remontam aos anos 1940, com Osamu Tezuka — o "Deus do Mangá" — que revolucionou a narrativa visual com obras como Astro Boy e Hi no Tori. Antes dele, já existiam as "kibyoshi" (livros ilustrados do século XVIII), mas Tezuka criou a linguagem visual que conhecemos hoje: close-ups dramáticos, linhas de velocidade, expressões exageradas.

**Principais gêneros:**
- **Shonen** (jovens masculinos): ação, amizade, superação. Ex: Naruto, Dragon Ball, One Piece
- **Shojo** (jovens femininas): romance, relacionamentos, emoções. Ex: Sailor Moon, Fruits Basket
- **Seinen** (adultos masculinos): temas maduros, psicológicos. Ex: Berserk, Vagabond, Vinland Saga
- **Josei** (adultos femininas): romance realista, slice of life. Ex: Nana, Chihayafuru
- **Isekai** (outro mundo): protagonista transportado para mundo fantástico. Ex: Re:Zero, Sword Art Online

**Autores que você precisa conhecer:**
Akira Toriyama (Dragon Ball), Eiichiro Oda (One Piece — o mangá mais vendido da história), Kentaro Miura (Berserk), Naoki Urasawa (Monster, 20th Century Boys) e Hajime Isayama (Attack on Titan).

**Como funciona o mercado:**
Mangás são publicados semanalmente ou mensalmente em revistas antológicas (Shonen Jump, Sunday, Magazine). Os capítulos são coletados em volumes tankobon. No Brasil, temos editoras como JBC, Panini e NewPOP trazendo os títulos oficialmente.

## O que funcionou

- Persona de "dono de loja" adicionou contexto de mercado brasileiro (JBC, Panini, NewPOP) que não apareceria num prompt genérico
- Profundidade histórica e técnica adequada ao nível de especialista declarado no role
- Nenhuma ambiguidade com fruta ou alfaiataria — desambiguação total com uma linha

## O que não funcionou

- Nada relevante para o objetivo de demonstração

## Ajustes que eu fiz

- Apenas a persona foi trocada em relação aos Prompts 2 e 3
- O duplo papel na persona ("especialista" + "dono de loja") enriqueceu a resposta com perspectiva de mercado

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Fecha o ciclo de 4 prompts com a mesma pergunta e 4 respostas radicalmente diferentes. A demonstração é completa: o role não apenas desambigua — ele calibra profundidade, tom e perspectiva (técnica vs. comercial vs. histórica).

## O que eu faria diferente

Testar o impacto de um role duplo vs. simples ("dono de loja" vs. apenas "especialista") para medir se a segunda função na persona realmente adiciona informação de mercado ou se o modelo infere isso sozinho pelo contexto.
