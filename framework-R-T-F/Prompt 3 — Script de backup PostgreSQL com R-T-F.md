# Experimento 03 — Script de backup PostgreSQL com R-T-F

**Data:** 2026-05-09
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Automação

## Contexto

Solicitação de script Bash de backup automático para PostgreSQL. O Role posiciona a IA como DBA Senior, o Task especifica os requisitos técnicos (compactação gzip, rotação de 7 dias, log) e o Format define a estrutura do script com cabeçalho e variáveis configuráveis no topo.

## Prompt usado

```markdown
# Role
Você é um DBA senior especializado em PostgreSQL e automação de rotinas operacionais.

# Task
Crie um script Bash de backup automático para um banco PostgreSQL, incluindo compactação com gzip, rotação de backups antigos (manter últimos 7 dias) e log de execução.

# Format
Script Bash comentado, com cabeçalho explicando uso e variáveis configuráveis no topo do arquivo.
```

## Resposta da IA

A IA gerou um script Bash completo, organizado e funcional. O cabeçalho documenta uso e pré-requisitos. As variáveis configuráveis estão agrupadas no topo. O script usa `pg_dump | gzip` com verificação de erro via `PIPESTATUS`, rotação com `find -mtime` e log com timestamp em todas as entradas.

## Output completo da IA

```bash
#!/bin/bash
# =============================================================================
# backup_postgresql.sh — Backup automático com compactação e rotação
#
# Uso: ./backup_postgresql.sh
#      Recomendado via cron: 0 2 * * * /opt/scripts/backup_postgresql.sh
#
# Pré-requisitos: pg_dump, gzip instalados; acesso ao banco configurado
#                 via .pgpass ou variável PGPASSWORD
# =============================================================================

# ---------------------------------------------------------------------------
# Variáveis configuráveis
# ---------------------------------------------------------------------------
DB_HOST="localhost"
DB_PORT="5432"
DB_USER="postgres"
DB_NAME="meu_banco"
BACKUP_DIR="/var/backups/postgresql"
RETENTION_DAYS=7
LOG_FILE="/var/log/postgresql_backup.log"

# ---------------------------------------------------------------------------
# Funções auxiliares
# ---------------------------------------------------------------------------
log() {
  echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# ---------------------------------------------------------------------------
# Início do backup
# ---------------------------------------------------------------------------
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')
BACKUP_FILE="${BACKUP_DIR}/${DB_NAME}_${TIMESTAMP}.sql.gz"

mkdir -p "$BACKUP_DIR"

log "Iniciando backup do banco '${DB_NAME}' em '${BACKUP_FILE}'"

pg_dump -h "$DB_HOST" -p "$DB_PORT" -U "$DB_USER" "$DB_NAME" \
  | gzip > "$BACKUP_FILE"

if [ "${PIPESTATUS[0]}" -eq 0 ]; then
  log "Backup concluído com sucesso: ${BACKUP_FILE}"
else
  log "ERRO: Falha ao gerar backup. Verifique as credenciais e conectividade."
  exit 1
fi

# ---------------------------------------------------------------------------
# Rotação: remove backups mais antigos que RETENTION_DAYS
# ---------------------------------------------------------------------------
log "Iniciando rotação — removendo backups com mais de ${RETENTION_DAYS} dias"
find "$BACKUP_DIR" -name "${DB_NAME}_*.sql.gz" -mtime +${RETENTION_DAYS} -exec rm -f {} \;
log "Rotação concluída."
```

## O que funcionou

- O cabeçalho inclui instrução de uso e exemplo de linha cron — pronto para produção
- Variáveis agrupadas no topo facilitam adaptação sem leitura do script inteiro
- Uso correto de `PIPESTATUS[0]` para detectar falha no `pg_dump` mesmo em pipeline com gzip
- Função `log()` reutilizável com timestamp em todas as mensagens
- Rotação filtra apenas arquivos do banco configurado (`${DB_NAME}_*.sql.gz`), evitando exclusão acidental
- Nenhuma reformulação necessária

## O que não funcionou

- O script não envia alerta (e-mail ou webhook) em caso de falha — ficou apenas no log local
- Não inclui verificação de integridade do arquivo gerado (ex: `gzip -t`)
- Sem tratamento de lock: se dois backups rodarem simultaneamente, podem haver conflitos no log

## Ajustes que eu fiz

- Nenhum ajuste no prompt foi necessário
- Em produção, complementaria o Task com: "em caso de falha, envie alerta via e-mail para o DBA responsável" e "valide a integridade do arquivo gerado com gzip -t"

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Script funcional, seguro e organizado desde a primeira resposta. O Role de DBA Senior fez a IA escolher `PIPESTATUS` (detalhe técnico que seria omitido por um prompt genérico) e o Format garantiu a estrutura de variáveis configuráveis que torna o script reutilizável sem edição do código.

## O que eu faria diferente

Adicionaria ao Task os requisitos de alertas e validação de integridade, que são práticas obrigatórias em ambientes de produção. Também especificaria no Format a necessidade de um bloco de tratamento de erros com exit codes distintos para facilitar integração com monitoramento externo.
