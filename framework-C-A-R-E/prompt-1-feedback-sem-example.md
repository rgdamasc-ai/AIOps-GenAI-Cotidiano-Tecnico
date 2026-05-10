# Prompt 1 — Feedback de desempenho SEM Example

**Cenário:** Feedback semestral para uma engenheira plena com Context, Action e Result, mas sem Example. Resultado vem em formato genérico, sem tom consistente com a empresa.

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo.
```

---

# Experimento 01 — Feedback de desempenho SEM Example

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Ciclo de avaliação semestral de uma engenheira plena em time de engenharia SaaS B2B.
Prompt estruturado com Context, Action e Result, mas sem campo Example.

## Prompt usado

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo.
```

## Resposta da IA

A IA gerou um feedback estruturado nas 3 seções solicitadas. O conteúdo é tecnicamente correto, mas o tom é genérico — "pilares de confiabilidade", "excelência técnica consistente" — sem personalidade definida. As metas dos Próximos Passos são vagas ("concluir um curso ou leitura") e não replicam o formato objetivo que seria esperado no contexto da empresa.

## Output completo da IA

**Pontos Fortes:**

A engenheira demonstrou excelência técnica consistente ao longo deste semestre. Ela cumpriu todos os prazos comprometidos nas sprints e manteve cobertura de testes acima da média do time em todos os projetos que participou. Sua capacidade de entrega é um dos pilares de confiabilidade do nosso time.

**Áreas de Desenvolvimento:**

A principal área de desenvolvimento é a comunicação com stakeholders de produto. Observamos que ela tem evitado participar de reuniões de alinhamento e, quando interage com o PM, tende a responder com linguagem excessivamente técnica, o que dificulta o entendimento e a tomada de decisão por parte de quem não tem contexto técnico.

Sugestões acionáveis:
- Praticar a tradução de decisões técnicas em impacto de negócio antes de reuniões
- Preparar um resumo em linguagem simples para reuniões com produto
- Participar ativamente das reuniões de alinhamento, mesmo que como ouvinte inicialmente

**Próximos Passos:**
- Participar de no mínimo 80% das reuniões de alinhamento com produto no próximo ciclo
- Em cada interação com o PM, incluir pelo menos uma frase sobre o impacto do trabalho para o usuário final
- Concluir até o final do próximo trimestre um curso ou leitura sobre comunicação para perfis técnicos

## O que funcionou

- Estrutura das 3 seções foi respeitada fielmente
- Identificação correta da área de desenvolvimento (comunicação com produto)
- Sugestões acionáveis razoáveis foram geradas sem pedir

## O que não funcionou

- Tom corporativo genérico — "excelência técnica consistente", "pilares de confiabilidade"
- Ausência de exemplos concretos nos Pontos Fortes (pedidos no prompt, não entregues)
- Meta de "Próximos Passos" vaga ("concluir um curso") sem prazo específico ou critério mensurável
- Nenhuma personalização ao contexto de SaaS B2B

## Ajustes que eu fiz

- Nenhum ajuste — output de primeira rodada sem reformulação
- O framework C-A-R sem E se mostrou insuficiente para ancorar o tom esperado

## Nota (1 a 5)

**Nota:** 3/5

**Justificativa:** A IA seguiu a estrutura corretamente, mas sem o Example o output ficou genérico, sem tom consistente e com metas pouco mensuráveis. Útil como rascunho inicial, mas exigiria reformulação significativa antes de usar com o time de RH.

## O que eu faria diferente

Incluir um Example de feedback anterior para ancorar o tom, o nível de especificidade e o formato das metas — exatamente o que o Prompt 2 testa. Sem esse aterramento, o modelo adota um tom "padrão corporativo" que provavelmente não representa a cultura do time.
