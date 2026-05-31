---
nome: Auditoria de Dockerfile Python
descricao: Audita um Dockerfile de aplicação Python e entrega recomendações priorizadas de segurança e performance com correções prontas para colar
versao: 1.0.0
ferramenta usada: Claude Code 
modelo: Opus 4.8
tags: [docker, segurança, performance, python, containers]
inputs: []
---

# Auditoria de Dockerfile Python

## Objetivo

Auditar um Dockerfile completo de aplicação Python (Flask, FastAPI, Django, workers Celery, scripts CLI, jobs batch) e produzir uma lista priorizada por severidade (CRÍTICO → ALTO → MÉDIO → BAIXO) de recomendações acionáveis sobre imagem base, usuário de runtime, gestão de dependências, estrutura de camadas, segredos, superfície de ataque, performance e operação — cada item acompanhado de uma correção Dockerfile pronta para colar.

## Quando usar

- Ao revisar o Dockerfile de um serviço Python antes de subir para produção.
- Para hardening de containers (execução não-root, supply chain, redução de superfície de ataque).
- Para otimizar tempo de build e tamanho de imagem (cache de camadas, multi-stage, variáveis Python).
- Em code review de PRs que alteram empacotamento ou containerização.

## Exemplo de uso

Cole o conteúdo do Dockerfile no bloco indicado ao final do prompt (seção `## Dockerfile a Analisar`) e envie. A saída segue um formato fixo: **Resumo da Auditoria**, **Recomendações Priorizadas** (ordenadas por severidade, com linha afetada, problema, impacto e correção Dockerfile) e **Pontos Positivos**.

## Limitações conhecidas

- O Dockerfile é colado em um bloco de comentário HTML no fim do prompt, não em um placeholder `{{...}}` — por isso `inputs` fica vazio.
- A análise é estática e baseada no julgamento do modelo: não executa scanners reais (Trivy/Snyk/Hadolint) nem valida CVEs ao vivo, e o próprio prompt instrui a não citar identificadores de CVE sem certeza.
