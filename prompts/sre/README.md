# DevOps

Prompts voltados a **infraestrutura, automação e operação** de sistemas: pipelines de CI/CD, containers, orquestração, provisionamento, observabilidade, confiabilidade e segurança operacional.

## Escopo

Entram aqui prompts relacionados a:

- Pipelines de CI/CD (GitHub Actions, GitLab CI, ArgoCD etc.).
- Containers e orquestração (Docker, Kubernetes, Helm).
- Infraestrutura como código (Terraform, Ansible).
- Provedores de nuvem (AWS, GCP) e seus recursos.
- Observabilidade (logs, métricas, tracing, alertas, dashboards).
- Confiabilidade, SRE, postmortems e análise de incidentes.
- Segurança operacional (hardening, secrets, políticas de acesso).

## Fora de escopo

- Escrita de código de aplicação → usar `desenvolvimento/`.
- Conteúdo educacional sobre DevOps (aulas, artigos, vídeos) → usar `criacao-conteudo/`.

## Prompts

- [auditar-dockerfile-python](./auditar-dockerfile-python/) — Audita um Dockerfile de aplicação Python e entrega recomendações priorizadas de segurança e performance com correções prontas para colar.
- [replicar-bucket-s3-cross-account](./replicar-bucket-s3-cross-account/) — Gera passo a passo via AWS Console para replicar um bucket S3 entre contas com acesso restrito ao consumidor e hardening de segurança nos dois buckets.
