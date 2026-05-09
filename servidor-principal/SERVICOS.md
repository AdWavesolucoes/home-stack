# Serviços Instalados e em Execução

> **Gerado em:** 2026-05-09  
> **Servidor:** DietPi @ 192.168.0.100

---

## Índice

1. [Serviços Systemd](#serviços-systemd)
2. [Containers Docker — Em execução](#containers-docker--em-execução)
3. [Containers Docker — Parados](#containers-docker--parados)
4. [Stacks Docker Compose](#stacks-docker-compose)
5. [Ollama — Modelos de IA Local](#ollama--modelos-de-ia-local)
6. [Serviços Python Nativos](#serviços-python-nativos)
7. [Volumes Docker](#volumes-docker)
8. [Redes Docker](#redes-docker)

---

## Serviços Systemd

| Serviço | Status | Descrição |
|---|---|---|
| `bluetooth.service` | ativo | Bluetooth |
| `bt-watchdog.service` | ativo | Watchdog dongle Bluetooth USB |
| `containerd.service` | ativo | Runtime de containers (Docker) |
| `cron.service` | ativo | Agendador de tarefas |
| `dbus.service` | ativo | D-Bus system message bus |
| `docker.service` | ativo | Docker Engine |
| `dropbear.service` | ativo | Servidor SSH leve (porta 22) |
| `mdmonitor.service` | ativo | Monitor de array RAID (md0) |
| `metrics-api.service` | ativo | API de métricas para dashboard ESP32 |
| `nmbd.service` | ativo | Samba NMB daemon (NetBIOS) |
| `ollama.service` | ativo | Servidor Ollama LLM local (porta 11434) |
| `pihole-FTL.service` | ativo | Pi-hole DNS + bloqueio de anúncios |
| `smbd.service` | ativo | Samba SMB daemon (portas 139/445) |
| `tailscaled.service` | ativo | Agente VPN Tailscale |
| `telegram-bot.service` | ativo | Bot Telegram "UpServidor" (monitoramento) |
| `unbound.service` | ativo | DNS recursivo local (upstream do Pi-hole) |
| `vsftpd.service` | ativo | Servidor FTP (porta 21) |

---

## Containers Docker — Em execução

### open-webui
| Campo | Valor |
|---|---|
| Imagem | `ghcr.io/open-webui/open-webui:main` |
| Porta | `8085` → `8080` (interno) |
| Volume | Docker volume `open-webui` |
| URL | `http://aichat.lan` / `http://192.168.0.100:8085` |
| Descrição | Interface web para LLMs — conectado via OpenAI API |
| Saúde | healthy |

### nextcloud
| Campo | Valor |
|---|---|
| Imagem | `nextcloud:latest` |
| Porta | `8888` → `80` |
| Volumes | `/mnt/nextcloud-raid/data`, `/mnt/nextcloud-raid/config`, `/mnt/nextcloud-raid/apps` |
| URL | `http://nextcloud.lan` / `http://192.168.0.100:8888` |
| Rede | `nextcloud_nextcloud_net` |
| Dependência | `nextcloud-db` (MariaDB) |
| Compose | `/opt/nextcloud/docker-compose.yml` |

### nextcloud-db
| Campo | Valor |
|---|---|
| Imagem | `mariadb:11` |
| Porta | interna (3306, não exposta) |
| Volume | `/mnt/nextcloud-raid/db` |
| Rede | `nextcloud_nextcloud_net` |
| Compose | `/opt/nextcloud/docker-compose.yml` |

### homeassistant
| Campo | Valor |
|---|---|
| Imagem | `ghcr.io/home-assistant/home-assistant:stable` |
| Rede | `host` (acesso direto, porta 8123) |
| Volume | `/opt/stacks/homeassistant/config` |
| URL | `http://192.168.0.100:8123` |
| Compose | `/opt/stacks/homeassistant/docker-compose.yml` |
| Recursos | Câmera Yoosee RTSP, sensores ESP32, automações |

### portainer
| Campo | Valor |
|---|---|
| Imagem | `portainer/portainer-ce:latest` |
| Portas | `9000` (HTTP), `9443` (HTTPS), `8000` (edge) |
| Volume | `portainer_data` |
| URL | `http://portainer.lan` / `https://192.168.0.100:9443` |

### npm (Nginx Proxy Manager)
| Campo | Valor |
|---|---|
| Imagem | `jc21/nginx-proxy-manager:latest` |
| Portas | `80` (HTTP), `81` (admin), `443` (HTTPS) |
| Volumes | `npm_data`, `npm_letsencrypt` |
| URL Admin | `http://192.168.0.100:81` |
| Compose | `/opt/npm/docker-compose.yml` |

### mosquitto
| Campo | Valor |
|---|---|
| Imagem | `eclipse-mosquitto:2` |
| Portas | `1883` (MQTT), `9001` (MQTT WebSocket) |
| Config | `/root/docker/mosquitto/config/` |
| Compose | `/root/docker/mosquitto/docker-compose.yml` |
| Descrição | Broker MQTT para ESP32 e PC Monitor |

### metrics-api
| Campo | Valor |
|---|---|
| Imagem | `metrics-api-metrics-api` (build local) |
| Porta | `5000` |
| Volume | `/var/run/docker.sock` (read-only), PID host |
| Compose | `/root/esp32-monitor/metrics-api/docker-compose.yml` |
| Descrição | API Python que expõe métricas do servidor para ESP32 |

### pc-monitor-webapp
| Campo | Valor |
|---|---|
| Imagem | `pc-monitor-webapp` (build local) |
| Porta | `3000` |
| Rede | `monitor-net` |
| Compose | `/root/pc-monitor-webapp/docker-compose.yml` |
| Descrição | Dashboard web de monitoramento do PC via MQTT |

### plex
| Campo | Valor |
|---|---|
| Imagem | `lscr.io/linuxserver/plex:latest` |
| Rede | `host` (porta 32400) |
| Volumes | `/opt/plex/config`, `/mnt/B8B615FAB615BA38/fILMES` (filmes), `/mnt/B0CEF741CEF6FE82` (torrents) |
| URL | `http://plex.lan:32400/web` |
| Compose | `/opt/plex/docker-compose.yml` |

---

## Containers Docker — Parados

### qbittorrent
| Campo | Valor |
|---|---|
| Imagem | `lscr.io/linuxserver/qbittorrent:latest` |
| Status | Exited (parado há ~2 semanas) |
| Portas | `8081` (WebUI), `6881` (BitTorrent) |
| Download dir | `/mnt/B0CEF741CEF6FE82/media/downloads` |
| Compose | `/root/stacks/qbittorrent/docker-compose.yml` |

---

## Stacks Docker Compose

### Stack: Nextcloud (`/opt/nextcloud/`)

| Serviço | Imagem | Descrição |
|---|---|---|
| `nextcloud` | `nextcloud:latest` | Aplicação web (porta 8888) |
| `nextcloud-db` | `mariadb:11` | Banco de dados |

> ⚠️ `docker-compose.yml` contém senhas hardcoded — **não commitar** versão original.  
> Use a cópia sanitizada em `docker-compose/nextcloud.yml`

---

### Stack: Home Assistant (`/opt/stacks/homeassistant/`)

| Campo | Valor |
|---|---|
| Imagem | `ghcr.io/home-assistant/home-assistant:stable` |
| Rede | host mode |
| Config | `/opt/stacks/homeassistant/config/` |
| Integrações ativas | Câmera Yoosee (RTSP), sensores ESP32 via MQTT, go2rtc |

> ⚠️ `configuration.yaml` contém credenciais RTSP da câmera — **não commitar** config/

---

### Stack: Watchtower (`/opt/stacks/watchtower/`)

| Campo | Valor |
|---|---|
| Imagem | `containrrr/watchtower:1.7.1` |
| Função | Atualização automática de containers com label `watchtower.enable: "true"` |
| Intervalo | A cada 6 horas (21600s) |
| Notificação | Telegram (token hardcoded no compose) |

> ⚠️ `docker-compose.yml` contém token do Telegram hardcoded — **não commitar** versão original.  
> Use a cópia sanitizada em `docker-compose/watchtower.yml`

---

### Stack: Mosquitto (`/root/docker/mosquitto/`)

MQTT broker para ESP32 e PC monitor.

| Campo | Valor |
|---|---|
| Imagem | `eclipse-mosquitto:2` |
| Portas | 1883 (MQTT), 9001 (WebSocket) |
| Config | `./config/mosquitto.conf` |

---

### Stack: Portainer + NPM (standalone, `/opt/npm/`, gerenciado via Portainer)

---

### Stack: Plex (`/opt/plex/`)

Servidor de mídia local.

| Campo | Valor |
|---|---|
| Imagem | `lscr.io/linuxserver/plex:latest` |
| Rede | host mode |
| Filmes | `/mnt/B8B615FAB615BA38/fILMES` (HDD Seagate 466GB) |
| Torrents | `/mnt/B0CEF741CEF6FE82` (SSD Netac 363GB) |
| Hardware accel | `/dev/dri` mapeado |

> ⚠️ `PLEX_CLAIM` no compose é token de claim (uso único, provavelmente expirado)

---

### Stacks Instaladas Mas Não Ativas

| Stack | Localização | Observação |
|---|---|---|
| paperclip | `/opt/paperclip/` | Container paperclip — Parado |
| homepage | `/opt/homepage/` | Dashboard (porta 3005) |
| traccar | `/opt/traccar/` | Rastreamento GPS (porta 8083) |
| kiwix | `/opt/kiwix/` | Servidor wiki offline (porta 18181) |
| spotify-embed | `/opt/spotify-embed/` | Player embed (porta 8399) |
| mkcert-web | `/opt/mkcert-web/` | Interface CA certificados locais (porta 8088) |
| code-server | `/root/code-server/` | VS Code no browser (porta 8888 — conflito!) |
| qbittorrent | `/root/stacks/qbittorrent/` | Cliente torrent (parado) |
| minecraft | `/opt/minecraft/` | Servidor Minecraft |
| prometheus | `/opt/prometheus/` | Coleta de métricas (volumes existem) |

---

## Ollama — Modelos de IA Local

**Serviço:** `ollama.service` (systemd) — porta `11434`  
**Integração:** Open WebUI (`aichat.lan`)

| Modelo | Tamanho | Última modificação |
|---|---|---|
| `deepseek-coder-v2:16b-lite-instruct-q2_K` | 6.4 GB | 7 dias atrás |
| `deepseek-coder:1.3b` | 776 MB | 9 dias atrás |

> Modelos ocultos no Open WebUI enquanto aguardam upgrade de hardware (RTX 3060 prevista para ago/2026)

---

## Serviços Python Nativos (systemd)

### telegram-bot (`/opt/telegram-bot/`)

| Campo | Valor |
|---|---|
| Serviço | `telegram-bot.service` |
| Script | `/opt/telegram-bot/bot.py` |
| Runtime | Python 3 (venv em `/opt/telegram-bot/`) |
| Função | Bot de monitoramento do servidor via Telegram |
| Reinício | automático (`always`, 10s delay) |

### metrics-api (`/opt/metrics-api/`)

| Campo | Valor |
|---|---|
| Serviço | `metrics-api.service` |
| Script | `/opt/metrics-api/metrics_api.py` |
| Porta | `8080` |
| Função | API Python que expõe métricas (CPU, RAM, disco, Docker) para ESP32 |

---

## Outras Ferramentas / Serviços

### Pi-hole + Unbound

| Campo | Valor |
|---|---|
| Pi-hole | `pihole-FTL.service`, porta 53 e :8082 (admin) |
| Unbound | DNS recursivo local — upstream do Pi-hole, porta 5335 |
| Blocklist | StevenBlack/hosts |
| Admin URL | `http://dns.lan/admin` |

### Tailscale VPN

| Campo | Valor |
|---|---|
| IP do servidor | `100.69.59.102` |
| Função | Acesso remoto seguro (mesh VPN) |
| DNS pendente | Configurar como nameserver na tailnet (admin.tailscale.com) para resolução `.lan` |

### Samba

| Campo | Valor |
|---|---|
| Share `servidor` | Raiz `/` do servidor |
| Acesso | Windows: `\\192.168.0.100\servidor` |

### vsftpd (FTP)

| Campo | Valor |
|---|---|
| Porta | 21 |
| Root local | `/mnt/dietpi_userdata` |
| Chroot | Desabilitado (acesso livre) |
| Anônimo | Desabilitado |

### Dropbear SSH

| Campo | Valor |
|---|---|
| Porta | 22 |
| Tipo | Servidor SSH leve (DietPi padrão) |

---

## Volumes Docker

| Volume | Usado por |
|---|---|
| `open-webui` | Open WebUI |
| `portainer_data` | Portainer |
| `npm_data` | Nginx Proxy Manager |
| `npm_letsencrypt` | Nginx Proxy Manager (certificados) |
| `ollama_data` | Ollama |
| `grafana_data` | Grafana (volumes existem, container não ativo) |
| `prometheus_data` | Prometheus (volumes existem, container não ativo) |

---

## Redes Docker

| Rede | Driver | Subnet | Usado por |
|---|---|---|---|
| `bridge` (docker0) | bridge | 172.17.0.0/16 | padrão |
| `docker_gwbridge` | bridge | 172.18.0.0/16 | Docker Swarm |
| `npm_default` | bridge | 172.19.0.0/16 | NPM, Paperclip |
| `nextcloud_nextcloud_net` | bridge | 172.22.0.0/16 | Nextcloud + DB |
| `monitor-net` | bridge | 172.24.0.0/16 | pc-monitor, metrics-api |
| `mosquitto_default` | bridge | 172.23.0.0/16 | Mosquitto |
| `metrics-api_default` | bridge | 172.25.0.0/16 | metrics-api |
| `watchtower_default` | bridge | 172.21.0.0/16 | Watchtower (down) |
