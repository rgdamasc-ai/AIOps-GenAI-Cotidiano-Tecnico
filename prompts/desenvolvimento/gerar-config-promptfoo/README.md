---
nome: Gerador de Config promptfoo
descricao: Gera um arquivo promptfooconfig.yaml completo para validar qualquer prompt, traduzindo critérios em linguagem natural para asserts do promptfoo (Gemini ou Anthropic)
versao: 1.0.0
ferramenta usada: Agnóstico (Claude / Gemini / ChatGPT)
modelo: Agnóstico
tags: [promptfoo, testes, validação-de-prompts, yaml, qa]
inputs: []
---

# Gerador de Config promptfoo

## Objetivo

Receber a descrição de um prompt e dos comportamentos que ele precisa garantir e devolver um arquivo `promptfooconfig.yaml` completo, válido e pronto para `promptfoo eval`. O prompt é agnóstico ao domínio: serve para validar qualquer prompt, traduzindo critérios em linguagem natural para os `tests` e `assert` certos (determinísticos como `regex`/`contains` ou avaliados por modelo como `llm-rubric`/`similar`).

## Quando usar

- Ao criar ou alterar um prompt e querer fixar seu comportamento esperado com testes automatizados.
- Para montar rapidamente uma suíte de regressão de prompts sem decorar a sintaxe do promptfoo.
- Para gerar variações de teste (múltiplos `vars`, casos limite e adversários) a partir de uma descrição informal.
- Quando precisar trocar o provider de avaliação entre Gemini 2.5 Flash (padrão) e algum modelo da Anthropic.

## Exemplo de uso

Preencha o bloco de comentário HTML da seção `## O Que Validar` ao final do prompt com: (1) o prompt sob teste (caminho `file://...` ou texto colado) e suas variáveis, (2) as regras que a saída deve garantir, (3) opcionalmente o provider desejado e (4) cenários específicos a cobrir. A saída é **apenas** o `promptfooconfig.yaml` dentro de um bloco ```yaml, com `description`, `prompts`, `providers`, `defaultTest` e `tests` prontos para salvar na raiz e rodar.

Por padrão o YAML sai com o provider determinístico:

```yaml
providers:
  - id: google:gemini-2.5-flash
    config:
      temperature: 0.0
```

Pedindo explicitamente Anthropic, o provider vira `anthropic:messages:claude-sonnet-4-6` (ajuste o id do modelo conforme a versão atual).

## Limitações conhecidas

- A entrada é colada em um bloco de comentário HTML no fim do prompt, não em um placeholder `{{...}}` — por isso `inputs` fica vazio.
- A qualidade dos `assert` depende de quão precisa é a descrição em "O Que Validar": critérios vagos geram checagens fracas.
- O conjunto de tipos de `assert` documentado no prompt é uma referência curada, não a lista exaustiva do promptfoo; recursos muito novos podem não estar cobertos.
- Asserts avaliados por modelo (`llm-rubric`, `similar`, `factuality`) têm custo e latência de chamada de LLM e introduzem não-determinismo — use-os só quando regra determinística não capturar o critério.
