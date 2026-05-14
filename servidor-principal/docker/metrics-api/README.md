# Metrics API

API REST que agrega métricas do servidor (CPU, RAM, disco, containers) para consumo por dashboards externos, incluindo o ESP32.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `http://IP:5000` | API REST |

---

## Quick Start

```bash
docker compose up -d
```

---

## Endpoints

| Endpoint | Descrição |
|----------|-----------|
| `GET /metrics` | Métricas gerais do sistema |
| `GET /containers` | Status dos containers Docker |
| `GET /health` | Health check |

---

## Observações

- Monta o socket Docker (`/var/run/docker.sock`) para listar e monitorar containers
- `pid: host` — compartilha namespace de PID com o host, permitindo ao `psutil` enxergar todos os processos
- Imagem construída localmente a partir do `Dockerfile` no repositório `esp32-monitor/metrics-api`
