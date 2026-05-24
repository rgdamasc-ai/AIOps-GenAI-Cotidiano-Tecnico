# Experimento 03 — Role de hortifruti: pergunta sobre manga

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Sonnet 4.6
**Categoria:** Engenharia de Prompt

## Contexto

Mesma pergunta do Prompt 1 ("Me fale sobre manga") com role de especialista em frutas tropicais de hortifruti. Terceiro cenário do grupo de demonstração do componente Role — mostra como a mesma pergunta gera um domínio completamente diferente dependendo da persona.

## Prompt usado

```
Você é um especialista em frutas tropicais que trabalha num hortifruti.

Me fale sobre manga.
```

## Resposta da IA

O modelo entrou no domínio de fruticultura: listou variedades (Tommy Atkins, Palmer, Espada, Haden, Ataulfo), detalhando sabor, textura e uso de cada uma. Cobriu safra por região (Nordeste out-jan, Sudeste dez-mar), critérios de seleção no ponto de venda, armazenamento e valor nutricional. Tom conversacional e prático, adequado ao contexto de varejo alimentar.

## Output completo da IA

Manga é uma das rainhas das frutas tropicais! Deixa eu te contar o essencial:

**Variedades principais no Brasil:**
- **Tommy Atkins:** a mais comum nas prateleiras. Casca avermelhada, polpa firme, boa durabilidade pós-colheita. Sabor menos intenso que outras variedades.
- **Palmer:** grande, adocicada, com menos fibra. Favorita para exportação e consumo in natura.
- **Espada:** alongada, amarela, polpa mais mole e fibrosa. Sabor marcante, popular no Nordeste.
- **Haden:** casca vermelha com pintas amarelas, sabor equilibrado entre doce e ácido.
- **Ataulfo (ou Manteiga):** pequena, sem fibra, polpa cremosa. Excelente para sorvetes e vitaminas.

**Safra e disponibilidade:**
A safra principal vai de outubro a janeiro no Nordeste (Juazeiro/BA e Petrolina/PE são os maiores polos). No Sudeste, pico entre dezembro e março.

**Como escolher e armazenar:**
- Pressione levemente: manga madura cede um pouco sem amassar
- Evite manchas escuras na casca — sinal de batida ou podridão
- Manga verde amadurece fora da geladeira em 2-3 dias; já madura, dura até 5 dias na geladeira

**Valor nutricional:** rica em vitamina C, A (betacaroteno), potássio e fibras. 100g têm cerca de 60 kcal.

## O que funcionou

- Role direcionou imediatamente para variedades, safra e armazenamento — informações úteis para quem compra ou vende a fruta
- Tom informal e acessível condizente com o ambiente de hortifruti
- Detalhe regional (Juazeiro/BA, Petrolina/PE) demonstra profundidade de conhecimento ativado pelo role

## O que não funcionou

- Nada relevante para o objetivo de demonstração

## Ajustes que eu fiz

- Mesma estrutura do Prompt 2 — apenas a persona foi trocada (alfaiate → especialista em frutas tropicais)
- Nenhuma outra modificação no prompt

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Confirmação perfeita do padrão: a troca de role produziu uma resposta radicalmente diferente para a mesma pergunta, agora focada em fruticultura prática. Contraste com Prompts 1 e 2 é o ponto pedagógico central.

## O que eu faria diferente

Especificar a região do hortifruti ("localizado em São Paulo") e o perfil do cliente ("majoritariamente famílias") para refinar ainda mais as recomendações de variedade e safra disponível localmente.
