# Prometheus

Coleta e armazena métricas do servidor e dos containers.

---

## docker run

```bash
docker run -d \
  --name prometheus \
  --restart always \
  -p 9090:9090 \
  -v /opt/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro \
  -v prometheus_data:/prometheus \
  prom/prometheus:latest
```

## Configuração

Arquivo: `/opt/prometheus/prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["prometheus:9090"]

  - job_name: "node"
    static_configs:
      - targets: ["172.17.0.1:9100"]

  - job_name: "cadvisor"
    static_configs:
      - targets: ["192.168.0.100:8080"]
```

### Targets

| Job | Endereço | Descrição |
|-----|----------|-----------|
| `prometheus` | `prometheus:9090` | Auto-monitoramento |
| `node` | `172.17.0.1:9100` | Node Exporter (host via bridge Docker) |
| `cadvisor` | `192.168.0.100:8080` | cAdvisor |

> **Nota:** O Node Exporter escuta em `172.17.0.1:9100` (IP do host na bridge Docker) em vez de `0.0.0.0:9100`, limitando o acesso à rede interna Docker.

---

## Acesso

Interface web disponível em `http://<IP>:9090` ou `https://metrics.lan`.

## Dados

Os dados ficam no volume Docker `prometheus_data`, persistindo entre reinicios e atualizações do container.
