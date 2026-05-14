# Grafana

Visualização de métricas e dashboards. Consome dados do Prometheus.

> Faz parte da [observability stack](../observability/). Suba pelo `docker-compose.yml` da pasta `observability/`.

---

## Acesso

| URL | Descrição |
|-----|-----------|
| `https://grafana.lan` | Acesso via proxy reverso (NPM) |
| `http://IP:3001` | Acesso direto |

Credenciais: usuário `Lucas` — senha via variável de ambiente (`GF_SECURITY_ADMIN_PASSWORD`).

---

## Dashboards importados

| Dashboard | Descrição |
|-----------|-----------|
| Node Exporter Full | Métricas completas do host |
| cAdvisor Exporter | Métricas por container Docker |

Os JSONs dos dashboards ficam em `../observability/grafana/dashboards/` para reimportação rápida.

---

## docker run

```bash
docker run -d \
  --name grafana \
  --restart always \
  -p 3000:3000 \
  -v grafana_data:/var/lib/grafana \
  grafana/grafana:latest
```

---

## Acesso

`https://grafana.lan` ou `http://<IP>:3000`

---

## Data Source

Configurar o Prometheus como data source:

- URL: `http://prometheus:9090`
- (ambos estão na mesma rede bridge Docker)

---

## Dashboards recomendados

Importar via ID no Grafana:

| ID | Nome |
|----|------|
| `1860` | Node Exporter Full |
| `193` | Docker and system monitoring (cAdvisor) |

---

## Dados

O volume `grafana_data` persiste dashboards, data sources e configurações entre atualizações.
