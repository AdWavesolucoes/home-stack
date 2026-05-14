# Observability Stack

Stack completa de monitoramento: Prometheus, Grafana, cAdvisor e Node Exporter.

---

## Serviços incluídos

| Container | Porta | Descrição |
|-----------|-------|-----------|
| Prometheus | `9090` | Coleta e armazena métricas |
| Grafana | `3001` | Dashboards e visualização |
| cAdvisor | `8084` | Métricas por container Docker |
| Node Exporter | `9100` | Métricas do host (CPU, RAM, disco, rede) |

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://grafana.lan` | Grafana via proxy reverso |
| `http://IP:3001` | Grafana acesso direto |
| `http://IP:9090` | Prometheus |

Credenciais Grafana: usuário `Lucas` — senha configurada via `.env`.

---

## Quick Start

```bash
docker compose up -d
```

---

## Arquitetura

```
Host
 ├── Node Exporter (9100)   ← CPU, RAM, disco, rede, processos
 └── cAdvisor (8084)        ← CPU/RAM/net/IO por container
         │
         ▼
     Prometheus (9090)      ← scrape a cada 15s, retém 15 dias
         │
         ▼
      Grafana (3001)        ← dashboards importados
```

## Dashboards

| Dashboard | ID Grafana | Descrição |
|-----------|------------|-----------|
| Node Exporter Full | `1860` | Visão completa do host |
| cAdvisor Exporter | `14282` | Métricas por container |

Os JSONs dos dashboards ficam em `grafana/dashboards/` para reimportação rápida.

---

## Configuração do Prometheus

O arquivo `prometheus/prometheus.yml` define os targets de scrape:

| Job | Target | Descrição |
|-----|--------|-----------|
| `prometheus` | `prometheus:9090` | Auto-monitoramento |
| `node` | `172.17.0.1:9100` | Node Exporter via bridge Docker |
| `cadvisor` | `cadvisor:8080` | cAdvisor (porta interna) |
