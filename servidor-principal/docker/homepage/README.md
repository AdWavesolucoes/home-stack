# Homepage

Dashboard de serviços com visão centralizada de todos os containers e links rápidos.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://home.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:3005` | Acesso direto |

---

## Quick Start

```bash
docker compose up -d
```

---

## Configuração

Os arquivos de configuração ficam em `./config/`:

| Arquivo | Descrição |
|---------|-----------|
| `services.yaml` | Lista de serviços exibidos |
| `settings.yaml` | Configurações gerais |
| `widgets.yaml` | Widgets de informação (clima, data, etc.) |
| `bookmarks.yaml` | Links rápidos |

---

## Integração com Docker

O socket `/var/run/docker.sock` é montado em modo somente leitura para exibir o status dos containers em tempo real.
