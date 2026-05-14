# Nextcloud

Nuvem privada self-hosted para armazenamento, sincronização de arquivos e colaboração.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://nextcloud.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:8888` | Acesso direto |

---

## Quick Start

```bash
cp .env.example .env
# edite .env com suas senhas
docker compose up -d
```

## Variáveis de Ambiente

Crie um arquivo `.env` baseado no `.env.example`:

| Variável | Descrição |
|----------|-----------|
| `MYSQL_ROOT_PASSWORD` | Senha root do MariaDB |
| `MYSQL_PASSWORD` | Senha do usuário `nextcloud` no banco |

---

## Volumes

| Caminho no host | Descrição |
|-----------------|-----------|
| `/mnt/nextcloud-raid/data` | Dados dos usuários |
| `/mnt/nextcloud-raid/config` | Configurações do Nextcloud |
| `/mnt/nextcloud-raid/apps` | Apps instalados |
| `/mnt/nextcloud-raid/db` | Banco de dados MariaDB |

> Os dados ficam em um RAID montado separadamente do sistema — disco dedicado para dados.

---

## Serviços incluídos

- `nextcloud` — aplicação principal (porta 8888)
- `nextcloud-db` — banco de dados MariaDB 11
