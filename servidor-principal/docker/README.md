# Docker

Orquestração de containers no servidor principal.

---

## Containers

| Container | Documentação |
|-----------|-------------|
| cadvisor | [cadvisor/README.md](cadvisor/README.md) |
| grafana | [grafana/README.md](grafana/README.md) |
| node-exporter | [node-exporter/README.md](node-exporter/README.md) |
| npm | [npm/README.md](npm/README.md) |
| plex | [plex/README.md](plex/README.md) |
| portainer | [portainer/README.md](portainer/README.md) |
| prometheus | [prometheus/README.md](prometheus/README.md) |
| qbittorrent | [qbittorrent/README.md](qbittorrent/README.md) |
| watchtower | [watchtower/README.md](watchtower/README.md) |

---

## Stack de Monitoramento

Prometheus, Node Exporter, cAdvisor e Grafana formam o stack de observabilidade:

```
Node Exporter  ←─ métricas do host (CPU, RAM, disco, rede)
cAdvisor       ←─ métricas dos containers Docker
        │
        ▼
   Prometheus  ←─ coleta e armazena as métricas
        │
        ▼
    Grafana    ←─ dashboards e visualização
```

## Atualização Automática

O **Watchtower** verifica e atualiza automaticamente as imagens dos containers marcados com a label:

```
com.centurylinklabs.watchtower.enable=true
```

---

## Gestão

O **Portainer** oferece interface web para gerenciar todos os containers, imagens e volumes.

Acesso: `https://portainer.lan` ou `http://<IP>:9000`
