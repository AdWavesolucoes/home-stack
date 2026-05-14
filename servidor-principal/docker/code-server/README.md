# Code Server

VS Code rodando no navegador. Permite desenvolvimento remoto no servidor a partir de qualquer dispositivo.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:8443` | Acesso direto |

---

## Quick Start

```bash
cp .env.example .env
# edite .env com sua senha
docker compose up -d
```

## Variáveis de Ambiente

| Variável | Descrição |
|----------|-----------|
| `CODE_SERVER_PASSWORD` | Senha de acesso à interface |

---

## Volumes

| Caminho no host | Descrição |
|-----------------|-----------|
| `./config` | Configurações, extensões e preferências |
| `/home` | Acesso aos arquivos do servidor |

---

## Observações

- O diretório `/home` do host é montado para acesso completo aos arquivos do servidor
- As extensões instaladas persistem no volume `./config`
