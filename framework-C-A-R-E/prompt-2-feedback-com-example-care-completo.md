# Prompt 2 — Feedback de desempenho COM Example (C-A-R-E completo)

**Cenário:** Mesmo cenário com Example fornecido (feedback do ciclo passado). O modelo replica o tom direto, exemplos concretos e formato de metas mensuráveis.

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo. Tom construtivo e direto, sem jargão corporativo.

# Example
Aqui está um feedback que escrevi no ciclo passado para outro membro do time, no formato e tom que quero manter:

**Pontos Fortes:**
O João foi peça-chave na entrega do projeto de migração do monólito. Ele liderou a definição da arquitetura de microsserviços e conduziu 3 design reviews com o time. Destaque para a documentação dos ADRs — ficou clara e serviu de referência para decisões futuras.

**Áreas de Desenvolvimento:**
O João precisa desenvolver mais autonomia na priorização. Em duas situações no trimestre, ele esperou direcionamento meu para decidir entre tarefas que poderia ter priorizado sozinho. Sugestão: usar a matriz de impacto/esforço antes de escalar.

**Próximos Passos:**
- Liderar pelo menos 1 design review por sprint sem acompanhamento do tech lead (até março)
- Priorizar autonomamente tarefas de até 3 story points sem escalar (próximo ciclo)
```

---

# Experimento 02 — Feedback de desempenho COM Example (C-A-R-E completo)

**Data:** 2026-05-10
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Mesmo cenário do Experimento 01 — ciclo semestral para engenheira plena em SaaS B2B.
Desta vez com campo Example: feedback real escrito para outro membro do time (João) como âncora de tom e formato.

## Prompt usado

```markdown
# Context
Sou tech lead de um time de 6 engenheiros em uma empresa de SaaS B2B. Estamos no ciclo de avaliação semestral. Preciso dar feedback individual seguindo o formato que o RH definiu.

# Action
Elabore um feedback de desempenho para uma engenheira plena que entrega muito bem tecnicamente (sempre cumpre prazos, código com boa cobertura de testes) mas precisa melhorar a comunicação com stakeholders de produto — ela evita reuniões e responde de forma muito técnica quando o PM faz perguntas.

# Result
O feedback deve ter 3 seções: "Pontos Fortes" com exemplos concretos, "Áreas de Desenvolvimento" com sugestões acionáveis, e "Próximos Passos" com metas mensuráveis para o próximo ciclo. Tom construtivo e direto, sem jargão corporativo.

# Example
Aqui está um feedback que escrevi no ciclo passado para outro membro do time, no formato e tom que quero manter:

**Pontos Fortes:**
O João foi peça-chave na entrega do projeto de migração do monólito. Ele liderou a definição da arquitetura de microsserviços e conduziu 3 design reviews com o time. Destaque para a documentação dos ADRs — ficou clara e serviu de referência para decisões futuras.

**Áreas de Desenvolvimento:**
O João precisa desenvolver mais autonomia na priorização. Em duas situações no trimestre, ele esperou direcionamento meu para decidir entre tarefas que poderia ter priorizado sozinho. Sugestão: usar a matriz de impacto/esforço antes de escalar.

**Próximos Passos:**
- Liderar pelo menos 1 design review por sprint sem acompanhamento do tech lead (até março)
- Priorizar autonomamente tarefas de até 3 story points sem escalar (próximo ciclo)
```

## Resposta da IA

A IA replicou fielmente o tom direto e concreto do Example. O output usa números reais ("acima de 90%", "três situações"), evita jargão corporativo, e os Próximos Passos têm o mesmo formato objetivo do feedback do João — com critério mensurável e contexto humanizado ("pode começar apenas acompanhando").

## Output completo da IA

**Pontos Fortes:**

A Ana foi referência de confiabilidade técnica no semestre. Ela cumpriu 100% dos prazos comprometidos nas sprints e foi a engenheira com maior cobertura de testes do time — acima de 90% nos módulos que desenvolveu. Em dois momentos críticos do semestre, identificou gargalos de performance antes que virassem incidente de produção e resolveu sem precisar acionar o time.

**Áreas de Desenvolvimento:**

A Ana precisa desenvolver sua comunicação com o time de produto. Em três situações no semestre, ela cancelou ou evitou reuniões de alinhamento com o PM. Quando questionada sobre a feature de exportação de relatórios, respondeu com detalhes de implementação que não ajudaram na decisão — o PM precisou reformular a pergunta duas vezes para chegar à resposta que precisava. O ponto não é sobre conhecimento técnico, que é sólido, mas sobre traduzir esse conhecimento para quem toma decisões de produto.

**Próximos Passos:**
- Participar de pelo menos 3 reuniões de alinhamento com produto nas próximas 4 sprints (pode começar apenas acompanhando, sem precisar falar)
- Em cada resposta a uma pergunta do PM, iniciar com o impacto para o usuário antes dos detalhes técnicos (próximo ciclo)

## O que funcionou

- Tom direto replicado com precisão — sem jargão, sem eufemismos
- Exemplos concretos nos Pontos Fortes (números, situações específicas)
- Áreas de Desenvolvimento com episódio real e causa-raiz identificada
- Próximos Passos enxutos, mensuráveis e com detalhe humanizado (sugestão de "começar apenas acompanhando")

## O que não funcionou

- Nada material — output utilizável direto, sem necessidade de reformulação

## Ajustes que eu fiz

- Nenhum — output de primeira rodada

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** O Example foi decisivo. A IA não apenas seguiu a estrutura — ela absorveu o tom, o nível de granularidade dos exemplos e o padrão de metas dos Próximos Passos. A diferença em relação ao Experimento 01 é imediata e significativa.

## O que eu faria diferente

Nada a mudar neste prompt. A lição: quando o tom e o formato importam, um Example real é mais eficaz do que qualquer descrição textual de "tom construtivo e direto". Mostrar é mais preciso do que explicar.
