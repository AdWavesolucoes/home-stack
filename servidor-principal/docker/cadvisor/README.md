# cAdvisor

Exporta métricas de uso de recursos de cada container Docker para o Prometheus.

---

## docker run

```bash
docker run -d \
  --name cadvisor \
  --restart always \
  -p 8080:8080 \
  -v /:/rootfs:ro \
  -v /var/run:/var/run:ro \
  -v /sys:/sys:ro \
  -v /var/lib/docker/:/var/lib/docker:ro \
  gcr.io/cadvisor/cadvisor:latest
```

---

## Métricas disponíveis

- CPU por container
- Memória por container
- I/O de disco por container
- Tráfego de rede por container

Todas acessíveis no Prometheus via job `cadvisor` e visualizadas no Grafana.
