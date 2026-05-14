# Minecraft Server

Servidor Minecraft com Forge 1.16.5 para mods.

---

## Acesso

| Endereço | Porta |
|----------|-------|
| `IP:25565` | Conexão padrão |
| `100.69.59.102:25565` | Acesso via Tailscale (qualquer lugar) |

---

## Quick Start

```bash
docker compose up -d
```

Na primeira inicialização o Forge é instalado automaticamente. Aguarde o log:
```
[Server thread/INFO]: Done (Xs)! For help, type "help"
```

---

## Configurações

| Parâmetro | Valor |
|-----------|-------|
| Versão | 1.16.5 |
| Mod loader | Forge 36.2.39 |
| Memória | 6 GB |
| Online mode | Desativado (sem auth Mojang) |

---

## Gerenciamento

```bash
# Console interativo
docker attach minecraft

# Logs
docker logs -f minecraft
```

> Os dados do servidor (mundo, mods, configurações) ficam em `/opt/minecraft`.
