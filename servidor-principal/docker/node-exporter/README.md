# Node Exporter

Exporta métricas do host (CPU, memória, disco, rede, processos) para o Prometheus.

---

## docker run

```bash
docker run -d \
  --name node-exporter \
  --restart always \
  --network host \
  --pid host \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /:/rootfs:ro \
  --label com.centurylinklabs.watchtower.enable=true \
  prom/node-exporter:latest \
  --path.procfs=/host/proc \
  --path.sysfs=/host/sys \
  --path.rootfs=/rootfs \
  --web.listen-address=172.17.0.1:9100
```

---

## Observações de Segurança

- `--web.listen-address=172.17.0.1:9100` — escuta **apenas no IP da bridge Docker**, não em `0.0.0.0`. Isso impede acesso direto pela rede local, permitindo apenas que o Prometheus (que está na mesma bridge) colete as métricas.
- `--network host` e `--pid host` são necessários para o Node Exporter enxergar os recursos reais do host.
- Os mounts `/proc`, `/sys` e `/` são somente leitura (`ro`).
