---
nome: Replicação Segura de Bucket S3 Cross-Account
descricao: Gera passo a passo via AWS Console para replicar um bucket S3 entre contas com acesso restrito ao consumidor e hardening de segurança nos dois buckets
versao: 1.0.0
ferramenta usada: Claude
modelo: Opus 4.8
tags: [aws, s3, replicação, segurança, sre]
---

# Replicação Segura de Bucket S3 Cross-Account

## Objetivo

Gerar um passo a passo executável pela AWS Management Console que configura replicação contínua de um bucket S3 da conta A para um bucket na conta B, concede acesso de leitura/escrita restrito a um serviço consumidor em B e garante hardening de segurança em ambos os buckets (Block Public Access, SSE-KMS, TLS obrigatório, Object Ownership "Bucket owner enforced" e logging), com todo IAM/bucket policy/KMS policy em JSON de menor privilégio pronto para colar.

## Quando usar

- Ao configurar replicação cross-account de S3 com requisitos de segurança rígidos.
- Quando um workload em outra conta (ECS, Lambda, Glue, EC2) precisa consumir os objetos replicados via IAM Role.
- Para revisar/hardenizar buckets envolvidos em replicação sem introduzir regressões de segurança.
- Quando a execução será manual pela console (não IaC) e o SRE precisa de passos clicáveis + JSON exato.

## Exemplo de uso

Preencha os placeholders `{{...}}` (IDs de conta, nomes de bucket, regiões, ARNs de CMK, nomes das roles) e cole os dados de conta/bucket no bloco `## Dados da conta e Bucket`. A saída segue formato fixo: **Resumo da arquitetura**, **Pré-requisitos validados**, **Passo a passo** (cada passo com Onde / O que fazer / Por que / Validação e JSON quando aplicável), **Verificação ponta a ponta** e **Riscos residuais e recomendações**.

## Limitações conhecidas

- Cobre a configuração via Console; não substitui IaC versionado (Terraform/CloudFormation) para reprodutibilidade.
- A replicação não é retroativa — objetos pré-existentes exigem S3 Batch Replication (fora do escopo dos passos).
- Os JSONs gerados usam placeholders simbólicos; o SRE precisa substituí-los e validar ARNs/condições no ambiente real antes de aplicar.
- KMS cross-account é a causa mais comum de falha silenciosa: requer conferência das key policies nas duas contas.
