# Docker — Servidor Principal

Orquestração de todos os containers do servidor via Docker Compose.

---

## Serviços

| Container | Descrição | Acesso |
|-----------|-----------|--------|
| [adwave-landing](adwave-landing/) | Landing page AdWave | `http://IP:8091` |
| [cadvisor](observability/) | Métricas de containers | `http://IP:8084` |
| [code-server](code-server/) | VS Code no navegador | `http://IP:8443` |
| [grafana](observability/) | Dashboards de monitoramento | `https://grafana.lan` · `http://IP:3001` |
| [homeassistant](homeassistant/) | Automação residencial | `https://ha.lan` |
| [homepage](homepage/) | Dashboard de serviços | `https://home.lan` |
| [kiwix](kiwix/) | Biblioteca offline (Wikipedia etc.) | `http://IP:18181` |
| [metrics-api](metrics-api/) | API de métricas para ESP32 | `http://IP:5000` |
| [minecraft](minecraft/) | Servidor Minecraft Forge 1.16.5 | `IP:25565` |
| [mosquitto](mosquitto/) | Broker MQTT (IoT) | `IP:1883` |
| [nextcloud](nextcloud/) | Nuvem privada | `https://nextcloud.lan` |
| [node-exporter](observability/) | Métricas do host | `http://IP:9100` |
| [npm](npm/) | Nginx Proxy Manager (proxy reverso) | `http://IP:81` |
| [open-webui](open-webui/) | Interface web para LLMs (Ollama) | `https://aichat.lan` · `http://IP:8085` |
| [pc-monitor-webapp](pc-monitor-webapp/) | Monitor de PC via ESP32 + MQTT | `http://IP:3000` |
| [plex](plex/) | Servidor de mídia | `https://plex.lan` |
| [portainer](portainer/) | Gestão visual de containers | `https://portainer.lan` · `http://IP:9000` |
| [prometheus](observability/) | Coleta e armazenamento de métricas | `http://IP:9090` |
| [qbittorrent](qbittorrent/) | Cliente BitTorrent | `http://IP:8081` |
| [traccar](traccar/) | Rastreamento GPS | `http://IP:8083` |
| [watchtower](watchtower/) | Atualização automática de imagens | — |

---

## Arquitetura de Rede

```
Internet
    │
    ▼
  NPM (80/443)            ← proxy reverso + TLS (.lan)
    │
    ├── nextcloud.lan     → nextcloud:8888
    ├── grafana.lan       → grafana:3001
    ├── ha.lan            → homeassistant (host)
    ├── aichat.lan        → open-webui:8085
    ├── portainer.lan     → portainer:9000
    └── ...
```

## Stack de Observabilidade

```
Host
 ├── Node Exporter (9100)   ← CPU, RAM, disco, rede do host
 └── cAdvisor (8084)        ← recursos por container
         │
         ▼
     Prometheus (9090)      ← coleta e armazena métricas
         │
         ▼
      Grafana (3001)        ← dashboards e visualização
```

## Stack IoT

```
ESP32 / Sensores
    │  MQTT
    ▼
Mosquitto (1883)
    │
    ├── Home Assistant     ← automação
    ├── pc-monitor-webapp  ← dashboard do PC
    └── metrics-api        ← API REST para dashboards externos
```

## Atualização Automática

O **Watchtower** atualiza imagens dos containers com a label:
```yaml
labels:
  com.centurylinklabs.watchtower.enable: "true"
```
